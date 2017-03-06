---
layout: post
title: "Pulling from a private GitHub repo within a Travis build"
description: ""
category: 
tags: [travis,devops,ci]
---
{% include JB/setup %}

The easiest way to inline clone a github repo in a Travis build is something you might be interested in if your project is split across multiple repos, or has a configuration or data dependency that is a separate git repo. Whatever the reason, there are several courses of action available according to the Travis [Private Dependency docs](https://docs.travis-ci.com/user/private-dependencies/). 

I've found the easiest - for bundler scripts that include private repos as well, I should add - way is to follow their advice for creating a travis user in github. Enable read access for this user - whatever you've named it - then get the username and password for this user you've created and make a note of them. 

Then `gem install travis` locally to get the travis command line tools. After that, it's as simple as doing

    travis sshkey --generate -r <your-org>/<your-private-repo> --debug --pro

Then when prompted, supply your travis user github username and password, _NOT_ your personal username and password.

    Username: travis-ci-user-name
    Password for travis-ci-user-name: ****

If you're pulling in a private depedency via bundler or a build system, then good news - you're done. However, if you want to do something inline, like pull in a repo of fixture data to a relative path of `./data` from your current working directory, then you'll need to do a bit more.

Make a script named say, `pull_data.sh` in a `./travis` directory, and add the following to it:

<script src="https://gist.github.com/the-frey/07b51790fe225969e088b240fdd51015.js"></script>

Save, and add this to your `.travis.yml` - probably in your `before-script` block.

    - sudo chown -R travis ./travis/pull_data.sh
    - sudo chmod +x ./travis/pull_data.sh
    - ./travis/pull_data.sh

Boom, you're done. Private dependency pulled on the fly, outside of a build system, using an sshkey only for access to a single repo.