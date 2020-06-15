---
layout: post
title: "Functional Serverless Typescript"
description: "What can we learn from other functional langs?"
category:
tags: [typescript,serverless,functional programming]
---
{% include JB/setup %}

A caveat to begin - I'm not an experienced Typescript programmer, and while I've been enjoying using it, this is my first Typescript project. For the last eighteen months, and for a period of years before that where I was working primarily in Clojure - I've been used to a language that is strongly typed but dynamic, and it was this pain point in Javascript that initially drove the decision to move to Typescript.

My current project is a serverless backend which we've had to initially get written and deployed very fast (in 7 days) and then iterate from there quickly. We started in JS as it was the way we could move the fastest, but after the initial go-live, we quickly re-factored and re-wrote to TS.

## Project Structure

The rough way we're structuring our project is (some files not shown for security/relevance reasons):

```
.
├── serverless.yml
├── src
│   ├── common
│   │   ├── auth
│   │   │   ├── auth_header.ts
│   │   │   └── iam_policy.ts
│   │   ├── aws.ts
│   │   ├── db.ts
│   │   ├── email.ts
│   │   ├── emails
│   │   │   └── transformations.ts
│   │   ├── missions
│   │   │   ├── db.ts
│   │   │   ├── mission_types.ts
│   │   │   └── transformations.ts
│   │   ├── responses.ts
│   │   ├── token_users
│   │   │   ├── db.ts
│   │   │   └── transformations.ts
│   │   ├── users
│   │   │   ├── db.ts
│   │   │   ├── transformations.ts
│   │   │   └── volunteers.ts
│   │   └── utils.ts
│   └── handlers
│       ├── auth
│       │   ├── oidc.ts
│       │   └── token_users.ts
│       ├── emails
│       │   └── create.ts
│       ├── missions
│       │   └── create.ts
│       ├── token_users
│       │   ├── auth.ts
│       │   └── create.ts
│       ├── users
│       │   ├── create.ts
│       │   ├── delete.ts
│       │   ├── get.ts
│       │   └── update.ts
│       └── utils
│           ├── dump_users.ts
│           └── migrate.ts
└── tsconfig.json
```

So - there's a top-level `common` folder for all the code and functions that support the actions of the application.

The `handlers` folder contains all the endpoints for the app, which are loosely REST-ful.

Each individual handler basically marshals all its transformations and executes a side effect.

In this way, you might think of the `handlers` folder as broadly containing the side-effecting code.

For each entity or vertical slice that's reified in the handlers folder, there's a corresponding entry in `common`. Within this, there are usually two buffers:
- `transformations` - this contains the pure functions that marshall data and transform it for this slice or entity.
- `db` - this contains the side-effecting functions that hit the database or mutate state.

There's also some additional supporting elements, like lookups, or `mission_types` which basically contains an `enum` and supporting tooling to verify data within the system for a core entity.

A final interesting thing to note is that we use lambdas for supporting tasks, e.g. dumping the database, or migrating it. These tasks are usually invoked by automated processes in our AWS account, but we can invoke them locally using `sls invoke`, or `npm` scripts. This means that broadly we're using the same tooling all the way from laptop to prod.

## Code Style

The general code style is:

- Pure functions wherever possible
- `no-var` is set in our linting rules - `const` only
- Where something is an object, we wrap method operations in a function: 
  - ```typescript
      const getDbConn = (): any => {
        const client = new Client();
        client.connect();
        return client;
      };
    ```
  - We can then pass it around and close over operations like so:
    - ```typescript
      const getAllUsers = (client: Client): Promise<User[]> => {
        const allUsersQuery = 'SELECT * from users;';
        return client.query(allUsersQuery);
      };
      ```
- Most functions use interfaces to determine their inputs and outputs
  - Especially with the functions in the `transformations` namespaces, these interfaces offer a lot of security, similar to `schema` or `spec` in Clojure
- Most functions take either a scalar item or an array, so `string` or `string[]` or `DefinedInterface`, `DefinedInterface[]`
  - We use the underlying node `pg` driver directly, and parameterised queries take an array of arguments, so: 
    - We put an interface around this
    - We have various transformations into and out of this format
- Where possible, _side effects only happen in handlers_.
- All HTTP responses return a variant type in their `body` of `[<entity>, <payload>]`. On paper this means you can compose responses more effectively if you want to chain, `map` or `reduce` over them, and/or polymorphically dispatch in some way on an endpoint's return - which we have a use case for in `mission_types`.
  - An example of this might be as simple as `['project-name-here/status', 'Success - user created']` or as complicated as `['project-name-here/user', <user-entity-representation>]`

What this means is that almost every function is unit testable.

- The database functions use a local PG in a docker container to test side effects
- The `transformation` namespaces can test pure data in, data out, as they are generally referentially transparent
- With a local database, the endpoints can be tested in the same way as side-effecting database functions
  - Because they call closed-over functions for db access, and clean them up later, they can be treated as black boxes (this is a deviation from serverless orthodoxy around leaving connections open)

So far this has been pretty good for us - we've got a pretty comprehensive test suite that runs in under 20s (including database and e2e tests) and are working on master in a trunk-style development process. 90% of the time, master is deployable directly to prod.

### End-to-end example

So, time for a quick example. I've changed the code to remove references to some additional security measures and changed some types to `any` because I couldn't include all the supporting stuff.

Obviously our project is not called `project-name-here` either.

To make it more simple I've changed the entity and how access works to numeric ids and keys - that's again not how the app works as that would expose potential security issues.

I've also annotated it a bit.

```typescript
// handler namespace

'use strict';
import * as _ from 'lodash';
import * as responses from '../../common/responses';
import * as db from '../../common/db';
import * as uDb from '../../common/users/db';
import * as uXfms from '../../common/users/transformations';

export const usersGet = async (event): Promise<any> => {
  const client = await db.getDbConn();

  try {
    const userId = event.pathParameters.id;

    const getUserRes = await uDb.getUserById(client, [userId]);
    const user = getUserRes.rows[0];

    if (_.isNil(user)) {
      await db.closeDbConn(client);
      return responses.respond404('project-name-here/error', 'User does not exist');
    }

    await db.closeDbConn(client);

    const userResponse = uXfms.dbUserToCamelCase(user);

    return responses.respond200('project-name-here/user', userResponse);

  } catch (err) {
    await db.closeDbConn(client);
    console.log(err);

    return responses.respond500('project-name-here/error', 'Something went wrong');
  }
};
```

```typescript
// responses.ts

interface Headers {
  'Access-Control-Allow-Origin': string;
  'Access-Control-Allow-Credentials': boolean;
}

interface Response {
  headers: Headers;
  statusCode: number;
  body: string; // JSON stringified body
}

const variantResponse = (type, payload): string => {
  return JSON.stringify([type, payload]);
};

const respond200 = (type, payload, headers = {}): Response => {
  return {
    headers: _.merge(corsHeaders(), headers), // cors headers have some defaults and env
    statusCode: 200,
    body: variantResponse(type, payload)
  };
};
```

You can then test things pretty effectively, as mentioned. I won't show any DB tests as they are pretty standard stuff, but here's an extract from the handler test and the transformations tests:

```typescript
// handler tests namespace

import * as db from '../../../src/common/db';
import * as uDb from '../../../src/common/users/db';
import * as handlers from '../../../src/handlers/users/get';

const truncateTablesQuery = `<truncation db query here>`;

afterEach(async () => {
  const client = await db.getDbConn();
  await client.query(truncateTablesQuery);
  await db.closeDbConn(client);
});

test('get handler returns an existing stub user', async () => {
  const client = await db.getDbConn();

  const userValues = [1, 'jeff.vader@example.com'];
  await uDb.insertStubUser(client, userValues);

  await db.closeDbConn(client);

  const expected = {
    'id': '1',
    'fullName': null,
    'phoneNumber': null,
    'email': 'jeff.vader@example.com',
  };

  const httpEvent = {
    pathParameters: {
      id: 1
    }
  };

  const response = await handlers.usersGet(httpEvent);
  const code = response.statusCode;

  expect(code).toEqual(200);

  const body = JSON.parse(response.body);
  const actual = body[1];

  expect(actual).toEqual(expected);
});

test('get handler returns 404 if user does not exist', async () => {
  const expected = 'User does not exist';

  const httpEvent = {
    pathParameters: {
      id: 1
    }
  };

  const response = await handlers.usersGet(httpEvent);
  const code = response.statusCode;

  expect(code).toEqual(404);

  const body = JSON.parse(response.body);
  const actual = body[1];

  expect(actual).toEqual(expected);
});

```

```typescript
// transformations test namespace

// this is basically what we get back from a DB select
const exampleUser = {
  id: 1,
  full_name: null,
  phone_number: null,
  email: 'jeff.vader@example.com'
};

test('dbUserToCamelCase converts a snake_case db representation to camelCase', () => {
  const expected = {
    id: 1,
    fullName: null,
    phoneNumber: null,
    email: 'jeff.vader@example.com'
  };

  const actual = uXfms.dbUserToCamelCase(exampleUser);
  expect(actual).toEqual(expected);
});
```

Just for completeness, the transformation function being tested looks like:

```typescript
// this isn't quite the reality, but it gives you the idea
interface CamelCaseUser {
  id: number;
  fullName: string;
  phoneNumber: string;
  email: string;
}

// convert the db representation to the json version
const dbUserToCamelCase = (dbRowObj): CamelCaseUser => {
  const {
    id,
    full_name,
    phone_number,
    email
  } = dbRowObj;

  const camelCaseVersion = {
    id: id,
    fullName: full_name,
    phoneNumber: phone_number,
    email: email
  };

  return camelCaseVersion;
};
```

[Here's a link to the code as a Gist](https://gist.github.com/the-frey/f6a03711f3f811056166d0626c577491).
