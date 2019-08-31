---
title: "This Week in KDE, Part 1 : GSoC, Kool Kommunity, Single/Double Click Bug"
date: "2018-05-07T02:49:22+03:00"
thumbnail: "/img/kdeplasma-1.jpg"
categories: ["KDE"]
tags: ["english", "kde"]
url: "this-week-in-kde-part-1"
---

### Cheap talk
I was planning to start contributing to KDE (Kool Desktop Environment 😎) project -because of my relapsing love to C++, Qt and KDE- and it's the time! I'll be a KDE hacker for the rest of my life 👨🏻‍💻

Last week, I was accepted to Google Summer of Code to work on the KDE project about improving input management. Click <a href="https://summerofcode.withgoogle.com/projects/#6683827406110720" target="_blank" rel="noopener">here</a> to see the summary of my proposal. At first, I was planning to implement custom mouse button support for KDE. We discussed the issue with <a href="https://blogs.kde.org/blogs/eike-hein" target="_blank" rel="noopener">Eike Hein</a> and <a href="https://pointieststick.wordpress.com/" target="_blank" rel="noopener">Nate Graham</a>. At the end, we decided that implementation of libinput for Xorg will be better to handle first and the adventure begun. Currently, I'm working on the project with 2 great mentors, <a href="https://pointieststick.wordpress.com/" target="_blank" rel="noopener">Nate Graham</a> and <a href="http://www.subdiff.de/" target="_blank" rel="noopener">Roman Gilg</a>. Also, I want to thanks to <a href="https://muhammetkara.com/" target="_blank" rel="noopener">Muhammet Kara</a> for his great supports. :)
</br><!----------------------CHEP TALK-->

### Show me the code
Currently I'm working on <a href="https://bugs.kde.org/show_bug.cgi?id=377310" target="_blank" rel="noopener">Bug 377310</a> and <a href="https://bugs.kde.org/show_bug.cgi?id=393547" target="_blank" rel="noopener">Bug 393547</a>. Bugs are basically about "Single/Double-click to open files and folders" setting. The setting is missing for libinput and it should be moved from "System Settings -&gt; Input Devices -&gt; Mouse" to "System Settings -&gt; Desktop Behavior -&gt; Workspace". Why ? Technically single/double-click setting is not part of the "Mouse" settings. When the user changes the setting from the "Mouse" settings, it also affects other input devices like touchpad because the option is related to the "Dolphin". Solution plan for the bugs consist of 3 main parts.

<strong>1.</strong> "Workspace" is written in <a href="https://techbase.kde.org/Development/Tutorials/KCM_HowTo">KCM (KConfig Modules)</a> and I should port it to QML <a href="https://api.kde.org/frameworks/kdeclarative/html/classKQuickAddons_1_1ConfigModule.html">KDeclarative::ConfigModule</a>.

<strong>2.</strong> Adding single/double-click options to "Workspace". This should be handled by changing "kdeglobals" config file. Xorg evdev implementation can be viewed from <a href="https://github.com/KDE/plasma-desktop/blob/6bb8cde96083f9bee6a20bbcffba7bd67c36c78b/kcms/input/backends/x11/evdev_settings.cpp" target="_blank" rel="noopener">here</a>. The new "Workspace" will be as the following;
![](/img/kde-workspace-kcm.png)

<strong>3.</strong> Cleaning the things about the setting in <a href="https://github.com/KDE/plasma-desktop/tree/6bb8cde96083f9bee6a20bbcffba7bd67c36c78b/kcms/input" target="_blank" rel="noopener">plasma-desktop/kcms/input</a> because the setting was there before.
</br><!---------------SHOW ME THE CODE-->

### Finally
I'll keep posting my progress at least twice a month. Hope to see you next week!
</br> <!------------------------FINALLY-->
