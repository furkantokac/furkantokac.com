---
title: "This Week in KDE, Part 4 : Mouse KCM, Bug Fixes!"
date: "2018-06-27T01:04:04+03:00"
thumbnail: "/img/kdeplasma-4.jpg"
categories: ["KDE"]
tags: ["english", "kde"]
url: "this-week-in-kde-part-4"
---

### Cheap talk
New Mouse KCM is available! Now, there are bugs to fix in my schedule so I'll more focuse on bug fixes then new features for sometime.

**New Mouse KCM:**
![](/img/kde-mouse-kcm.png)
</br><!----------------------CHEP TALK-->

### Show me the code
<strong>1.</strong> Mouse KCM redesing is published. The link is <a href="https://phabricator.kde.org/R119:e4ce025aa7065d09765211e1dd0433403b37a51b">here.</a>
<strong>2.</strong> Touchpad KCM QtQuickControls2 Conversion is published. The link is <a href="https://phabricator.kde.org/R119:6a4b5870fb2ff918df9e5b8f708e8039b394177e">here.</a>
<strong>3.</strong> Mouse KCM Pointer Speed Slider Improvement is published. The link is <a href="https://phabricator.kde.org/R119:e4ce025aa7065d09765211e1dd0433403b37a51b">here.</a> Also this patch fixes the <a href="https://bugs.kde.org/show_bug.cgi?id=395681">Bug 395681</a>.
<strong>4.</strong> Same thing done for Mouse KCM should be done also for Touchpad KCM. Patch is coming. The link is <a href="https://phabricator.kde.org/D13767">here.</a>
</br> <!---------------SHOW ME THE CODE-->

### Finally
Many BUGs to fix! My todolist (tofixlist :) ) is as following:

<strong>1.</strong> <a href="https://bugs.kde.org/show_bug.cgi?id=387153">Bug 387153</a> (libinput-touchpad-kcm-on-x11)
libinput-backend touchpad KCM only used on Wayland

<strong>2.</strong> <a href="https://bugs.kde.org/show_bug.cgi?id=387156">Bug 387156</a> (libinput-touchpad-kcm-click-method)
libinput touchpad KCM lacks support for click method ("areas" or "fingers") for buttonless touchpads

<strong>3.</strong> <a href="https://bugs.kde.org/show_bug.cgi?id=392709">Bug 392709</a> (libinput-touchpad-kcm-horiz-scroll)
Option to disable horizontal scrolling with Libinput touchpad interface

<strong>4.</strong> <a href="https://bugs.kde.org/show_bug.cgi?id=395348">Bug 395348</a> (Libinput backend)
enable/disable touchpad setting is gray out and unavailable since plasma 5.13

<strong>5.</strong> <a href="https://bugs.kde.org/show_bug.cgi?id=395351">Bug 395351</a>
Touchpad settings are disabled in Wayland

<strong>6.</strong> <a href="https://bugs.kde.org/show_bug.cgi?id=395401">Bug 395401</a> (evdev)
Mouse settings are not loaded up at login

<strong>7.</strong> <a href="https://bugs.kde.org/show_bug.cgi?id=395404">Bug 395404</a>
"Press left and right buttons for middle click" setting is not remembered under Wayland

Hope to see you next week! :)
</br> <!------------------------FINALLY-->
