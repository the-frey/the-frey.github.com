---
layout: post
title: "Showing Ruby Version in Agnoster Theme"
description: "I spend a fair bit of time in the terminal, so found this useful"
category: 
tags: [tip, quick, zsh]
---
{% include JB/setup %}

Here's a really quick post for a change. As I'm sure many others do, I use `zsh` as my main terminal, with `oh-my-zsh` to make the config a bit easier. I'm a big fan of the Agnoster theme, but miss having rvm information in my prompt. A bit of digging online, and here's a quick fix that will help by getting rvm and ruby information displayed on the right-hand side: 

1. Add `rvm` to the `plugins` line of your `~/.zshrc` file
2. At the bottom of your aliases, add an alias for `rvm-prompt` `alias rvm-prompt=$HOME/.rvm/bin/rvm-prompt`
3. Reload (you may not need this line, but if you do, put it below the above) `source $ZSH/oh-my-zsh.sh`
4. Add the following on the line below: `RPROMPT="%{$fg[blue]%}\$(~/.rvm/bin/rvm-prompt s i v g) %{$fg[yellow]%}[%*]%{$fg[default]%}"`

It's not hugely important what all the bits of that line are (and you can probably make an educated guess, in any case), but tweet me or look in the docs if you're curious. 

Happy hacking!