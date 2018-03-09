---
layout: post
title: "Clojure in a Serverless World"
description: "To JVM or not to JVM, that is the question..."
category: 
tags: [clojure,serverless,lambda]
---
{% include JB/setup %}

I'm not going to present anything here as my original thought when it's not, but just put up some links.

After thinking through the implications of Serverless/Lambda, I asked the following question on [r/Clojure](https://www.reddit.com/r/Clojure/comments/82tqzw/discussion_place_of_clojure_in_a_world_of/):

```
Genuinely interested here as I was at an AWS builder's day last week and the tooling is very nodejs focussed (and python I guess) so far as I can tell. Start up and warm up costs for the jvm too. Plus:

1) a lot of best practice in clojure land becomes implicit when you have to separate out front and backend because of using e.g. lambda to serve an API endpoint 
2) things like the serverless (framework) that looks to provide an abstraction over AWS, azure and gcp is also nodejs and for example looks like a decent way of being able to do backend agnostic stuff.

I was an early adopter of docker back in 2014 so I'm looking at this stuff and going "well if this is the future of large systems then how will it affect my ways of working?"

Just some things I've been pondering...

UPDATE:

When I say the serverless framework as opposed to pattern, I specifically mean this abstraction framework.
```

The serverless framework I referred to was [this one](https://github.com/serverless/serverless), FWIW.

So anyway, there's some great responses in that thread including some great battle-earned wisdom, for example this from [yogthos, the maintainer of Luminus and author of the excellent Web Development with Clojure book](https://github.com/yogthos):

> My team built an app using it that runs in our local kubernetes environment. The startup is pretty much instant, and the whole app takes under a 100 megs of memory at runtime, and packaging it using Docker ends up being pretty close to shipping an uberjar. The biggest pain in my opinion is the async nature of Node. While core.async helps, I still find the code is a lot more fiddly than the equivalent that you'd write on the JVM.

He also explained the use case of the app, and the pain points they experienced:

> we found that the mental overhead grew very quickly as we added more IO calls. The app is basically a simple CRUD app for collecting patient consents and surveys. Since it was very small scope we figured Node might be a good candidate for it.

> The app looks up patients by their medical number from a remote service to fill out their demographics info, then allows the physician and the patient to view and sign the consents, and runs a patient through a survey.

> Where things got messy was in cases where we needed to chain multiple IO operations together. For example, we'd save a consent, then need to generate a report from the consent surveys, and produce a PDF that went to a remote service. Even in a simple case where you have a few IO operations chained together, the async nature of Node would make things messy. Error reporting becomes especially problematic as you have to manually remember to pass errors around. If you forget then there's no indication of what went wrong.

> Another problem is that you need to be careful about running any CPU intensive code. For example, report generation will block the main thread, so as the size of your data grows, this operation can potentially block handling client requests. The solution on node seems to be to use multi-processing or to farm out to a separate service that does CPU bound work.

> This problem is entirely absent on the JVM, where everything can be handled synchronously in the request handling thread.

If you're anything like me, you're thinking, "wow, that's good to know." Thanks Dmitri for sharing - again, check out the full thread because there's also some good Component vs Mount takeaways.

But the main thing is there are some ace links, so I've reproduced them here, in case (I guess) Reddit goes away tomorrow, or something.

- [Fearless JVM Lambdas video](https://www.youtube.com/watch?v=GINI0T8FPD4)
- [Lumo: cross-platform Clojurescript](https://github.com/anmonteiro/lumo)
- [CLJS Lambda: a lein plugin for developing cljs projects on AWS Lambda](https://github.com/nervous-systems/cljs-lambda)
- [Portkey: expose any function as an AWS Lambda](https://github.com/portkey-cloud/portkey)

FWIW, Portkey is a great idea, and I really want to make a toy project with it and dig a little deeper. Hopefully I'll get a chance in the coming weeks.


