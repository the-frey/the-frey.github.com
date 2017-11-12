---
layout: post
title: "Setting up a zsh terminal with agnoster on OSX Sierra"
description: "It's been a while..."
category: 
tags: [zsh,oh-my-zsh,agnoster]
---
{% include JB/setup %}

I've just started a new job, and having not set up a new laptop in over four years, I've gotten a little rusty. So, if like me you use mac's Terminal.app and haven't changed it in well, ever, then you might find some issues on a new Sierra machine, particularly if you use the combo of zsh/oh-my-zsh and the agnoster theme like I do. 

So tl;dr - turns out if you install oh-my-zsh, switch it to the agnoster theme, add a patched powerline font, and add a solarized terminal profile the colours are still wrong, and it's because the solarized main repo hasn't been updated post-Yosemite.

The issue is referenced in [this pull request](https://github.com/altercation/solarized/pull/314) and the file in question can be found [here](https://github.com/jcberthon/solarized/blob/b8f7484fed98de37736b7c025cbcea7647204fe6/osx-terminal.app-colors-solarized/Solarized%20Dark%20Yosemite.terminal).

Download that file and import it into Terminal and bam! You're set.