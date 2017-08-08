---
layout: post
title: "Debugging with Emacs and CIDER"
description: "A quick example of debugging using CIDER and Emacs"
category: 
tags: [clojure,emacs,cider]
---
{% include JB/setup %}

Okay, before we go any further, let's just observe that cider - obviously - is an alcoholic drink, not a name for cloudy apple juice like some Americans seem to think. Cool? Cool.

Okay, so CIDER and Emacs is basically the de-facto standard for writing Clojure, and more recent versions come with a pretty ace debugger that's accessible via `#dbg`. To use it, wrap the expression you want to debug in it, like `#dbg(expr)` then eval the expression with `C-M-x` a couple of times to make sure your REPL gets the idea. For some reason in the past loading the whole file has not eval'd the reader macro properly.

Apologies about the contrived example, but here's how it might work -

<script src="https://gist.github.com/the-frey/5b73e5e45db38b7acaf569696b904fe7.js"></script>

NB: I typed that straight into Gist, so it's possible my parens aren't totally balanced. Oh, my kingdom for Paredit in Gist. Oh well.

Final thing - remember to take it out before you jar or push your project as this is a utility function only, and it will stop your project compiling properly.