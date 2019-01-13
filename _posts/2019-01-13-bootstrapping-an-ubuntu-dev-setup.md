---
layout: post
title: "Bootstrapping an Ubuntu dev setup"
description: "I've recently moved back to Ubuntu for my personal projects. Here's what I need to get a close-to-mac level of slickness."
category: 
tags: [ubuntu,mac,emacs]
---
{% include JB/setup %}

# Motivation

I'm not looking forward to having to pay £2500 for a mac that I can't manually upgrade, so I want the option of doing my day-to-day dev work on a moderately powerful ultrabook.

In this case, I've chosen a refurb 3rd gen i7 (5600) Lenovo Thinkpad Carbon X1. You can pick up a unit in immaculate condition for about £400.

It's got an HD screen and can easily run a 32" monitor. It's got an HDMI port, two USB connections and a fantastic keyboard with decent travel. It's also super thin and light.

I found that since I last used Ubuntu full-time (2012-13), it's come on leaps and bounds. Customising Gnome is super easy, and it looks great.

I had originally thought of jumping to Arc or even Arcolinux, but as I got most of a working setup going over just a lunchbreak, I figured Ubuntu was actually the one I was after.

___

# Basics

- On installation, encrypt the disk or partition
- Make sure your computer isn't sending any data home
- Stick an [EFF sticker](https://supporters.eff.org/shop/laptop-camera-cover-set) over the webcam
- If like me you're on 8GB of RAM (Lenovo 3rd Gen ThinkPad Carbon X1), then [increase your swap to 12G](https://bogdancornianu.com/change-swap-size-in-ubuntu/). There's also a [great DigitalOcean guide](https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-18-04) which also covers swappiness, a value you might want to tweak.
- Install Firefox and Chromium, so you have two browsers to test with
  - Add a password manager
  - Add EFF HTTPS Everywhere
  - Add EFF Privacy Badger
  - Add uBlock Origin
  - Add React Developer tools
- Generate an SSH key and add it to your GitHub, and/or any other services that need a public key. [Use this guide](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/#platform-linux)
- Install Slack from Ubuntu Software (hit `Super` and type that)

## Keyboard

Set the keyboard to `English, UK, Macintosh`.

### Keyboard shortcuts

I have a bunch of things that I set up to be a little like my mac. In some cases, these are things (like lock screen) which I actually set on my mac after custom setting them in Ubuntu in 2013. Ho hum.

Go to `Settings -> Devices -> Keyboard` and then change:

- Move to Workspace Above: `Ctrl+Right`
- Move to Workspace Below: `Ctrl+Left`
- Lock screen: `Ctrl-Escape`

___

# Look and feel

I'm biased coming from a mac, but I like my system to look good. 

I get annoyed by the dock/launcher always being present so first, go to system settings, and select the auto-hide option. You probably want to resize it too.

The second thing you're going to need is the Gnome Tweak Tool, now called `Tweaks`. 

```
$ sudo add-apt-repository universe
$ sudo apt install gnome-tweak-tool
```

Then hit `Super` and start typing `tweaks`. When it appears, hit Enter.

The first (and main thing) you're going to want to do is enable right-clicking using the touchpad (otherwise it's in my opinion a somewhat clunky two-finger tap). Go to 'Keyboard and Mouse' and under 'Mouse Click Emulation' select 'Area'. Some touchpads are more flaky with the area setting though, so try it for a bit and see what works for you.

## Theme

First off, I like the Arc Theme. As described [here](https://www.omgubuntu.co.uk/2017/11/best-gtk-themes-for-ubuntu), it's available from the Ubuntu Software Store as well.

If memory serves, I changed the top bar colour to fit the theme a bit more nicely, and possibly tinkered with Chromium a bit too.

## Icon pack

I use the OSX-like [La Capitaine](https://github.com/keeferrourke/la-capitaine-icon-theme) icons. They recommend the `X-Arc` Arc offshoot, which is no longer being developed, but can be found [here](https://gitlab.com/LinxGem33/X-Arc-White).

## Gnome extensions

Install the extensions tool:

```
$ sudo apt install gnome-shell-extensions
$ sudo apt install chrome-gnome-shell
```

Don't worry about the "chrome" bit if you're using FF, I think that's just a legacy thing.

Then, install the extension for your browser:

- [Firefox](https://addons.mozilla.org/en-US/firefox/addon/gnome-shell-integration/)
- [Chrome](https://chrome.google.com/webstore/detail/gnome-shell-integration/gphhapmejobijbbhgpjhcjognlahblep)

Then get:

- Dynamic transparency plugin [here](https://extensions.gnome.org/extension/1011/dynamic-panel-transparency/)
- System monitor plugin [here](https://extensions.gnome.org/extension/120/system-monitor/)

For system monitor, I display CPU, Memory, Network and temperature. The settings I have are:

- CPU: Display: Yes; Display style: Both; Graph Width 50; Show text: Yes.
- Memory: Yes; Display style: Both; Graph Width 50; Show text: Yes.
- Network: Display: Yes; Display style: Both; Graph Width 50; Show text: No.
- Temp: Display: Yes; Display style: Both; Graph Width 10; Show text: No.

___

# Fingerprint login

Follow the advice on this [question](https://askubuntu.com/questions/1049526/fingerprint-activation-on-ubuntu-18-04).

    $ sudo apt install -y fprintd libpam-fprintd

In `settings->users` there will now be an option for fingerprint login.

___

# Emacs setup

Kevin Kelly's PPA is definitely the least painful way that I've found. There's a [good thread](https://www.reddit.com/r/emacs/comments/8pcw5a/what_is_the_most_painless_way_to_install_emacs_26/) of alternatives on Reddit, if you're interested.

```
$ sudo add-apt-repository ppa:kelleyk/emacs
$ sudo apt-get update
$ sudo apt install emacs26
```

After grabbing [my emacs config](https://github.com/the-frey/emacs) from GitHub, I found that I needed to add the following to the top of my `init.el` file:

```
(defvar ido-cur-item nil)
(defvar ido-default-item nil)
(defvar ido-cur-list nil)
```

I also I think had to `M-x package install RET rainbow-delimiters`, but I'm not 100% on that one.

You need to set the face to a mono face that's powerline-compatible. Luckily, Ubuntu Mono will do the job here.

Hit `M-x customize RET` then in `Faces -> Basic Faces` find the `Default` option. Click to expand (or hit enter if you're in terminal), and change font family to `Ubuntu Mono`. Then hit `Apply and Save` and you should be good to go!

To get the excellent `joker` linter to work, I downloaded a linux version of the tool from [here](https://github.com/candid82/joker/releases) and then moved it to `/usr/local/bin`.

Job done.

___

# Sublime setup

First, grab sublime from the official PPA:

```
$ wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
$ sudo apt-add-repository "deb https://download.sublimetext.com/ apt/stable/"
```

Then install it.

```
$ sudo apt install sublime-text
```

This will link the `subl` command in the terminal for you.

Add your registration key to the application and it will remove the `UNREGISTERED` warning from the window chrome.

## Packages

First install sublime package manager [from here](https://packagecontrol.io/).

Then, install the following packages:

- Theme - Flatland
- GitGutter
- Google Spell Check
- paredit
- Python 3
- WordCount

## Settings

I like these settings in my user config:

<script src="https://gist.github.com/the-frey/5ce58d2d355837642ff9336801bf2437.js"></script>

## Key bindings

Some of these will be controversial, I'm sure, but these mean that my habit of moving workspaces with `CTRL-Left` and `CTRL-Right` on mac can be shuffled over to Ubuntu, and that the lack of an option key means I can still have easy-to-remember line shuffling tools.

On the whole, most sublime settings on Linux will be CTRL instead of option, but there are some exceptions. One of my most used Sublime features is `CTRL-Shift-L` over a selection region, so at least that _just works_.

<script src="https://gist.github.com/the-frey/35f51ea31e8c571fd8e5ba70acbb2a53.js"></script>

___

# Terminal setup

I use zsh with `oh-my-zsh`.

```
$ sudo apt install git-core zsh
$ sh -c “$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
$ sudo apt install fonts-powerline
```

Then, update the `.zshrc` config file...

Here's my base `.zshrc`:

```sh
# If you come from bash you might have to change your $PATH.
# export PATH=$HOME/bin:/usr/local/bin:$PATH

# Path to your oh-my-zsh installation.
  export ZSH="/home/the_frey/.oh-my-zsh"

# Set name of the theme to load --- if set to "random", it will
# load a random theme each time oh-my-zsh is loaded, in which case,
# to know which specific one was loaded, run: echo $RANDOM_THEME
# See https://github.com/robbyrussell/oh-my-zsh/wiki/Themes
ZSH_THEME="agnoster"

# Set list of themes to pick from when loading at random
# Setting this variable when ZSH_THEME=random will cause zsh to load
# a theme from this variable instead of looking in ~/.oh-my-zsh/themes/
# If set to an empty array, this variable will have no effect.
# ZSH_THEME_RANDOM_CANDIDATES=( "robbyrussell" "agnoster" )

# Uncomment the following line to use case-sensitive completion.
# CASE_SENSITIVE="true"

# Uncomment the following line to use hyphen-insensitive completion.
# Case-sensitive completion must be off. _ and - will be interchangeable.
# HYPHEN_INSENSITIVE="true"

# Uncomment the following line to disable bi-weekly auto-update checks.
# DISABLE_AUTO_UPDATE="true"

# Uncomment the following line to change how often to auto-update (in days).
# export UPDATE_ZSH_DAYS=13

# Uncomment the following line to disable colors in ls.
# DISABLE_LS_COLORS="true"

# Uncomment the following line to disable auto-setting terminal title.
# DISABLE_AUTO_TITLE="true"

# Uncomment the following line to enable command auto-correction.
# ENABLE_CORRECTION="true"

# Uncomment the following line to display red dots whilst waiting for completion.
# COMPLETION_WAITING_DOTS="true"

# Uncomment the following line if you want to disable marking untracked files
# under VCS as dirty. This makes repository status check for large repositories
# much, much faster.
# DISABLE_UNTRACKED_FILES_DIRTY="true"

# Uncomment the following line if you want to change the command execution time
# stamp shown in the history command output.
# You can set one of the optional three formats:
# "mm/dd/yyyy"|"dd.mm.yyyy"|"yyyy-mm-dd"
# or set a custom format using the strftime function format specifications,
# see 'man strftime' for details.
# HIST_STAMPS="mm/dd/yyyy"

# Would you like to use another custom folder than $ZSH/custom?
# ZSH_CUSTOM=/path/to/new-custom-folder

# Which plugins would you like to load?
# Standard plugins can be found in ~/.oh-my-zsh/plugins/*
# Custom plugins may be added to ~/.oh-my-zsh/custom/plugins/
# Example format: plugins=(rails git textmate ruby lighthouse)
# Add wisely, as too many plugins slow down shell startup.
plugins=(
  git
  zsh-autosuggestions
  heroku
  docker
  docker-compose
  rails
  rvm
)

source $ZSH/oh-my-zsh.sh

# Set personal aliases, overriding those provided by oh-my-zsh libs,
# plugins, and themes. Aliases can be placed here, though oh-my-zsh
# users are encouraged to define aliases within the ZSH_CUSTOM folder.
# For a full list of active aliases, run `alias`.
#
# Example aliases
alias zshconfig="subl ~/.zshrc"
alias wordcount="wc --words"
alias prtf=catdoc # print rtf
alias pbcopy='xclip -sel clip'
# alias ohmyzsh="mate ~/.oh-my-zsh"

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
# [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

```

**Important** - you now need to switch back to bash and make zsh your default shell.

```
$ bash
$ chsh -s $(which zsh)
```

You probably also want it setup on a solarized colour scheme.

I like to be able to copy output from the CLI, so I install `xclip`:

    $ sudo apt install xclip

cURL might not be installed yet:

    $ sudo apt-install curl

`htop` is useful:

    $ sudo apt-install htop

Install `catdoc` and `wordcount` if they aren't already.

I also add some custom aliases to my `~/.gitconfig`:

```
[alias]
        co = checkout
        wat = status
        in = add
        go = commit
```

___

# Programming languages

Obviously this is very specific to me. I don't do as much Ruby any more, so I'm not installing that right off the bat. If I did, I would do it with RVM.

## Node

Install Node using NVM [with this guide](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-18-04):

```
$ curl -sL https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh -o install_nvm.sh
$ bash install_nvm.sh
$ source ~/.profile
```

If any of the following steps fail, then you might need to check your `~/.zshrc` file, and add the lines:

```sh
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
```

Find the latest LTS using:

    $ nvm ls-remote

For me, that's listed as `10.15.0`. Okay then.

```
nvm install 10.15.0
nvm use 10.15.0
```

Let's check it's okay:

    $ node -v
    => v10.15.0

## Clojure

Install a JDK, in this case, the OpenJDK. [Here's a good guide](https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-on-ubuntu-18-04)

Then, to get Clojure (at time of writing, other instructions on the clojure site):

```
$ curl -O https://download.clojure.org/install/linux-install-1.10.0.411.sh
$ chmod +x linux-install-1.10.0.411.sh
$ sudo ./linux-install-1.10.0.411.sh
```

Ubuntu has a pretty up-to-date Leiningen in the package manager, so you can just [get it from there](https://launchpad.net/ubuntu/+source/leiningen-clojure), or you can install it using the `lein` script from the Leiningen website.

___

# Install Docker

Double check there are no old versions lurking:

    sudo apt-get remove docker docker-engine docker.io containerd runc

Get dependencies:

```
$ sudo apt-get update
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

From the Docker docs:

```
# Verify that you now have the key with the fingerprint 9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88, by searching for the last 8 characters of the fingerprint.
$ sudo apt-key fingerprint 0EBFCD88
```

Then install the beast:

```
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
$ sudo apt-get update
$ sudo apt-get install docker-ce
```

You can check it's running with:

    $ sudo docker container run hello-world

Phew.

___

# Writing

Get FocusWriter, and set it to the green text on black screen for maximum hackage.

___

# Dumb other stuff

I like eDEX-UI as a completely nutty way of working in the terminal occasionally.

Working with zsh and oh-my-zsh requires editing the `settings.json` file to add my battlestar theme, and move its port away from 3000, which is a pretty risky default port imo.

```
{"shell":"zsh","cwd":"/home/the_frey/projects","keyboard":"en-US","theme":"battlestar","port":1999}
```

Grab a `woff2` version of Ubuntu Mono, put it in the `fonts` folder of the eDEX-UI config folder and name it `ubuntu_mono.woff2`.

Then, save this in themes as `battlestar.json`:

<script src="https://gist.github.com/the-frey/823d3b49151a120b1592ffb77588bd75.js"></script>
