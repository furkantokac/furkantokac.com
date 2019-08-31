---
title: "This Week in KDE, Part 5 : Slider Bug Fix, Libinput UI on X11"
date: "2018-07-08T14:56:38+03:00"
thumbnail: "/img/kdeplasma-5.jpg"
categories: ["KDE"]
tags: ["english", "kde"]
url: "this-week-in-kde-part-5"
---

### Cheap talk
As most of the KDE people know, you can use Libinput on X11 but there are some issues with the combination of Libinput + X11 + Touchpad KCM. In KDE, when you use Libinput on X11, there will not be a special KCM support to handle Libinput settings. Fixing this issue is one of the main purpose of my Google Summer of Code adventure. And, the day has come! Our planning and discussions about the issue is done and the work is in progress. Let me give you some background information.

For touchpads, the situation is a little bit complicated. Touchpad has 2 possible display server choices in traditional PCs and different possibilities for input management libraries;

##### Xorg
1. Evdev
1. Synaptics
1. Libinput

##### Wayland
1. Libinput

So the KCM gets a little bit messy. Our plan is separating Wayland and Xorg KCMs into 2 different KCMs and showing the KCM to the user according to s/he's backend. This will lead to a simpler KCM and simpler means easier to hack. After that, X11 will say welcome to its new Libinput KCM support :) So basically the plan has 2 main parts. First part is on progress! We'll deploy all the patches soon.

While working on the Libinput patch, a bug about Mouse/Touchpad KCM's "Pointer Speed" slider is reported. The problem was that the slider was not totally accurate and we fixed the bug.
</br> <!----------------------CHEP TALK-->

### Show me the code
<strong>1.</strong> Mouse KCM Pointer Speed Slider Improvement patch is published. Commit is <a href="https://phabricator.kde.org/R119:13b35bd8025a4bcf399670c32ee20327b0ace392">here</a>.
<strong>2.</strong> Touchpad KCM Pointer Speed Slider Improvement patch is published. Commit is <a href="https://phabricator.kde.org/R119:86e674c6a2b9bd8c99b419b15f33c0bf898e7d54">here</a>.
</br> <!---------------SHOW ME THE CODE-->

### Finally
Now, my todolist is as following:
<strong>1.</strong> Separating Touchpad KCM into 2 different KCMs: Touchpad KCM (Wayland, kcm_touchpad) and TouchpadX KCM (X11, kcm_touchpadX). Also by this patch, Touchpad KCM will be rewritten in KConfigModule.
<strong>2.</strong> Some cleaning in TouchpadX KCM.
<strong>3.</strong> Adding Libinput KCM support to X. By this patch, <a href="https://bugs.kde.org/show_bug.cgi?id=387153">Bug 387153</a> will be fixed.

Hope to see you soon! :)
</br> <!------------------------FINALLY-->
