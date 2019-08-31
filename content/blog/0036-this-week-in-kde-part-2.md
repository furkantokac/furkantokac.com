---
title: "This Week in KDE, Part 2 : OYLG, Workspace KCM, Single/Double Click"
date: "2018-05-20T11:47:51+03:00"
thumbnail: "/img/kdeplasma-2.jpg"
categories: ["KDE"]
tags: ["english", "kde"]
url: "this-week-in-kde-part-2"
---

### Cheap talk
Last weekend, I went to İstanbul to attend <a href="https://ozguryazilimgunleri.org.tr/2018/">Özgür Yazılım ve Linux Günleri</a> (Free Software and Linux Days 2018) to represent LibreOffice. We had 3 presentations during the event about LibreOffice Development and The Open Document Format. We had booth setup with stickers, flyers, roll-up etc. These were all thanks to The Document Foundation's supports! You can find detailed information about the event from here : <a href="https://wiki.documentfoundation.org/Events/2018/OYLG2018">https://wiki.documentfoundation.org/Events/2018/OYLG2018</a>

**Summary of the event :)**
![](https://wiki.documentfoundation.org/images/5/5a/The_LibreOffice_and_the_GNOME_booths_in_OYLG18.jpg)

I came back from the event in Monday and since then, I have been working on the <a href="https://github.com/KDE/plasma-desktop/tree/master/kcms/workspaceoptions">Workspace KCM</a> rewrite and <a href="https://bugs.kde.org/show_bug.cgi?id=393547">single/double click bug fix</a> so just find time to write blog. <a href="https://phabricator.kde.org/D12936">Workspace KCM rewrite</a> and <a href="https://phabricator.kde.org/D12946">single/double click bug fix</a> are done and the code is pushed to the master. If you're on the dev unstable repo, and if you have updated your system in the last 4 days, just go to "System Settings > Desktop Behavior > Workspace" and check it. You should be using new Workspace KCM :) But there are more. I finished a new design for it yesterday and it'll be pushed soon, after some discussion with maintainers.
See the evolution of Workspace KCM :

**KCM Rewrite**
![](https://phabricator.kde.org/file/data/ql5y6fvd2pkdl5mxkbv6/PHID-FILE-d2rda27dwsxjctn5uvdd/Screenshot_20180517_014851.png)

**Single/double click bug fix**
![](https://phabricator.kde.org/file/data/tzfaehldtmuo22ksbcdr/PHID-FILE-jir4id5fnk75pz4nfpjt/workspacekcmv2.png)

**Kirigami redesign**
![](https://phabricator.kde.org/file/data/4k7l7cslm7dukdjx2ke6/PHID-FILE-3lflg3x4ji5tccuo2bpi/kirigamiworkspacekcm.png)
</br> <!----------------------CHEP TALK-->

### Show me the code
**1.** The old Workspace KCM is rewritten in QML. Commit is <a href="https://cgit.kde.org/plasma-desktop.git/commit/?id=856f58955aec5e26018cb83ad485f5492babc241">here.</a></br>
**2.** In Wayland, there was no "single/double click" options and in Xorg, it was under "Mouse KCM" but this doesn't make sense. When user change the setting, it doesn't affect only mouse, it affects whole system because it is not about input devices. So we discussed the issue and decided to move the setting to Workspace KCM. This also solved the issue for Wayland. In short, single/double click options is moved to Workspace KCM. Commit is <a href="https://cgit.kde.org/plasma-desktop.git/commit/?id=d3ef895516fed319956e4ba843206dd118b998f5">here</a>.</br>
**3.** Workspace KCM is redesign by using Kirigami and the code has same improvement. See the work from <a href="https://phabricator.kde.org/D12973">here (D12973)</a> and <a href="https://phabricator.kde.org/D12974">here (D12974)</a>.</br>
</br> <!---------------SHOW ME THE CODE-->

### Finally
These were very active days for me. I have been in lots of discussions, did have lots of conversations. Basically I understand how things going on in the background. Nate and Roman are great mentors, friends and the KDE is a great community.

Now, my todolist is as following:</br>
**1.** Rewrite Touchpad KCM in QtQuick.</br>
**2.** Rewrite Mouse KCM in QtQuick.

Hope to see you next week! :)
</br> <!------------------------FINALLY-->
