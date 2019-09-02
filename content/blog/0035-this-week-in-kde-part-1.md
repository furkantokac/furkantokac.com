---
title: "This Week in KDE, Part 1 : GSoC, Kool Kommunity, Single/Double Click Bug"
date: "2018-05-07T02:49:22+03:00"
thumbnail: "/img/kdeplasma-1.jpg"
categories: ["KDE"]
tags: ["English", "KDE", "FOSS Dev"]
url: "this-week-in-kde-part-1"
---

### Cheap talk

I was planning to start contributing to KDE (Kool Desktop Environment 😎) project -because of my relapsing love to C++, Qt and KDE- and it's the time!

Last week, I was accepted to Google Summer of Code to work on the KDE project about improving input management. Click [here](https://summerofcode.withgoogle.com/projects/#6683827406110720) to see the summary of my proposal. At first, I was planning to implement custom mouse button support for KDE. We discussed the issue with [Eike Hein](https://blogs.kde.org/blogs/eike-hein) and [Nate Graham][nate]. At the end, we decided that implementation of libinput for Xorg will be better to handle first and the adventure begun. Currently, I'm working on the project with 2 great mentors, [Nate Graham][nate] and [Roman Gilg][roman]. Also, I want to thanks to [Muhammet Kara](https://muhammetkara.com/) for his great supports. :)
<!----------------------CHEP TALK-->


### Show me the code

Currently I'm working on [Bug 377310](https://bugs.kde.org/show_bug.cgi?id=377310) and [Bug 393547](https://bugs.kde.org/show_bug.cgi?id=393547). Bugs are basically about "Single/Double-click to open files and folders" setting. The setting is missing for libinput and it should be moved from "System Settings -&gt; Input Devices -&gt; Mouse" to "System Settings -&gt; Desktop Behavior -&gt; Workspace". Why ? Technically single/double-click setting is not part of the "Mouse" settings. When the user changes the setting from the "Mouse" settings, it also affects other input devices like touchpad because the option is related to the "Dolphin". Solution plan for the bugs consist of 3 main parts.

**1.** "Workspace" is written in [KCM (KConfig Modules)](https://techbase.kde.org/Development/Tutorials/KCM_HowTo) and I should port it to QML [KDeclarative::ConfigModule](https://api.kde.org/frameworks/kdeclarative/html/classKQuickAddons_1_1ConfigModule.html). </br>
**2.** Adding single/double-click options to "Workspace". This should be handled by changing "kdeglobals" config file. Xorg evdev implementation can be viewed from [here](https://github.com/KDE/plasma-desktop/blob/6bb8cde96083f9bee6a20bbcffba7bd67c36c78b/kcms/input/backends/x11/evdev_settings.cpp). The new "Workspace" will be as the following;
![](/img/kde-workspace-kcm.png)
</br></br>
**3.** Cleaning the things about the setting in [plasma-desktop/kcms/input](https://github.com/KDE/plasma-desktop/tree/6bb8cde96083f9bee6a20bbcffba7bd67c36c78b/kcms/input) because the setting was there before.
<!---------------SHOW ME THE CODE-->


### Finally

I'll keep posting my progress at least twice a month.

Hope to see you next week!
<!------------------------FINALLY-->

[nate]: https://pointieststick.com/
[roman]: http://www.subdiff.de/
