---
layout: post
title: "Helidox | Corne | CRKBD build log"
description: "I just finished building my first DIY keyboard, the helidox. Here's how I got on."
category: 
tags: [keyboard,helidox,corne]
---
{% include JB/setup %}

![helidox](https://raw.githubusercontent.com/the-frey/the-frey.github.com/master/assets/images/helidox.JPG)

First up, it worked, woo! I'm actually typing this on the Helidox. Compared to the Ergodox EZ I have, it's slicker, smaller and - weirdly - easier to navigate. my theory about why this should be is two-fold; firsly, that the Helidox is smaller, so easier to get your head around; secondly, it's so different to what you're used to that you're forced to make a clean break and adapt.

So, without further ado, here's some of the stuff that I ran into during the build.

## Flashing issues from Ubuntu

Having recently declared OSX bankruptcy, I was trying to flash the boards from my now-trusty Lenovo Thinkpad Carbon... but this wasn't as smooth sailing as I had hoped.

Looking at the logs, I could see there was no effect when I plugged in the pro micro:

    $ tail -f /var/log/syslog

So I looked online, and decided the most likely culprit was a bad cable - I tried several cables with the log running and finally saw the controller recognised. Time to flash, then:

    $ util/docker_build.sh crkbd:default:avrdude

Unfortunately this didn't work. I've learned to be somewhat skeptical of Docker being able to execute complicated builds across the host and ports.

I installed the essentials on the host machine and tried again with `make`. To use this approach:

```
$ sudo apt-get update
$ sudo apt-get install gcc unzip wget zip gcc-avr binutils-avr avr-libc dfu-programmer dfu-util gcc-arm-none-eabi binutils-arm-none-eabi libnewlib-arm-none-eabi
$ make crkbd:default:avrdude
```

It blew up again, but this time because it didn't have `avrdude`.

$ sudo apt-get install avrdude

This time it worked. Hooray!

It's worth also mentioning that on subsequent flashes you might need to use sudo with the `make` command.

## Issues caused by me

Once I got the keyboard flashed and running, I found a few issues with my build.

## LEDs

When I booted it up, I found that all my front LEDs (the ~21 that go under the switches) appeared to be fine, but I had a number of issues with the underlighting ones. SMD and small pads makes for sad times.

In the end I found a destroyed trace was the problem under one - as the front LEDs are in series after the back ones, if something goes wrong with one of the fiddly ones you're hosed, unfortunately.

I've built a lot of stuff - pedals, studio gear, amps - and I feel like the difficulty of soldering the underglow SMD LEDs still came as a surprise. From looking on forums, it seems like I'm not the only one that had trouble.

## OLED screens

Uh, so make sure you jumper these. Somehow, like an idiot I jumpered one side and not the other. As my kit didn't come with sockets, I couldn't take it apart.

Luckily, the side that had issues was the one that just displays the 'Corne' logo, not the one that shows actually useful information like layer etc.

I will probably look at desoldering and fixing this today.

Which leads me to...

## Sockets

Socket your microcontrollers!

Seriously, I know you're confident enough to solder them okay, but socket them anyway, alright?!

Next time I will probably use [these sockets](https://uk.rs-online.com/web/p/pcb-pin-socket-strips/7020165/) which I believe will be compatible with the smaller pin sockets that are currently available - and possibly also the DIP sockets you can get for the ProMicro.

## N-Key rollover

N-key rollover is disabled by default because of issues on Windows. If you want paredit and emacs to be usable, you'll need to enable it. To do this:

- Fork the [QMK Repo](https://github.com/qmk/qmk_firmware)
- Change the `NKRO_ENABLE` variable in the `crkbd` build
- Re-build and re-flash from your fork

My fork is [here](https://github.com/the-frey/qmk_firmware), in case you're curious. I should probably pull my files out into a new keymap at some point, but for now it's over `default`. My fork, my rules.

## Arrow keys

I wanted to add the arrow keys to the lower layer, so this is simply a case of editing the `keymap.c` and assigning the following to the correct place:

- KC_LEFT
- KC_DOWN
- KC_UP
- KC_RIGHT

Note that as the Helidox/crkbd keymap has a macro in it, you can omit the `KC_` part.

## Pictures

I've thrown on some keycaps from my old Corsair K65 for now, but I'm waiting on some DSA Light Cycle caps I ordered. Once those materialise, I'll update this post with some pictures.

## Layout

This is the layout that I'm currently using; the three markings on each key indicate the layer they're on, with top being raise, middle being default and bottom being lower. Obviously the keys marked will behave as you'd expect with the shift key, i.e. `/` becomes `?`.

![helidox layout](https://raw.githubusercontent.com/the-frey/the-frey.github.com/master/assets/images/keyboard-layout.jpg)
