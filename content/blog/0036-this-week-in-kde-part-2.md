---
title: "This Week in KDE, Part 2 : OYLG, Workspace KCM, Single/Double Click"
date: "2018-05-20T11:47:51+03:00"
thumbnail: "/img/kdeplasma-2.jpg"
categories: ["KDE"]
tags: ["English", "KDE", "FOSS Dev"]
url: "this-week-in-kde-part-2"
---

### Cheap talk

Last weekend, I went to İstanbul to attend [Özgür Yazılım ve Linux Günleri](https://ozguryazilimgunleri.org.tr/2018/) (Free Software and Linux Days 2018) to represent LibreOffice. We had 3 presentations during the event about LibreOffice Development and The Open Document Format. We had booth setup with stickers, flyers, roll-up etc. These were all thanks to The Document Foundation's supports! You can find detailed information about the event from here : [https://wiki.documentfoundation.org/Events/2018/OYLG2018](https://wiki.documentfoundation.org/Events/2018/OYLG2018)

**Summary of the event :)**
![](https://wiki.documentfoundation.org/images/5/5a/The_LibreOffice_and_the_GNOME_booths_in_OYLG18.jpg)

I came back from the event in Monday and since then, I have been working on the [Workspace KCM](https://github.com/KDE/plasma-desktop/tree/master/kcms/workspaceoptions) rewrite and [single/double click bug fix](https://bugs.kde.org/show_bug.cgi?id=393547) so just find time to write blog. [Workspace KCM rewrite](https://phabricator.kde.org/D12936) and [single/double click bug fix](https://phabricator.kde.org/D12946) are done and the code is pushed to the master. If you're on the dev unstable repo, and if you have updated your system in the last 4 days, just go to "System Settings > Desktop Behavior > Workspace" and check it. You should be using new Workspace KCM :) But there are more. I finished a new design for it yesterday and it'll be pushed soon, after some discussion with maintainers.
See the evolution of Workspace KCM :

**KCM Rewrite**
![](https://phabricator.kde.org/file/data/ql5y6fvd2pkdl5mxkbv6/PHID-FILE-d2rda27dwsxjctn5uvdd/Screenshot_20180517_014851.png)

**Single/double click bug fix**
![](https://phabricator.kde.org/file/data/tzfaehldtmuo22ksbcdr/PHID-FILE-jir4id5fnk75pz4nfpjt/workspacekcmv2.png)

**Kirigami redesign**
![](https://phabricator.kde.org/file/data/4k7l7cslm7dukdjx2ke6/PHID-FILE-3lflg3x4ji5tccuo2bpi/kirigamiworkspacekcm.png)
<!----------------------CHEP TALK-->


### Show me the code

**1.** The old Workspace KCM is rewritten in QML. Commit is [here](https://cgit.kde.org/plasma-desktop.git/commit/?id=856f58955aec5e26018cb83ad485f5492babc241).</br>
**2.** In Wayland, there was no "single/double click" options and in Xorg, it was under "Mouse KCM" but this doesn't make sense. When user change the setting, it doesn't affect only mouse, it affects whole system because it is not about input devices. So we discussed the issue and decided to move the setting to Workspace KCM. This also solved the issue for Wayland. In short, single/double click options is moved to Workspace KCM. Commit is [here](https://cgit.kde.org/plasma-desktop.git/commit/?id=d3ef895516fed319956e4ba843206dd118b998f5).</br>
**3.** Workspace KCM is redesign by using Kirigami and the code has same improvement. See the work from [here (D12973)](https://phabricator.kde.org/D12973) and [here (D12974)](https://phabricator.kde.org/D12974).
<!---------------SHOW ME THE CODE-->


### Finally

These were very active days for me. I have been in lots of discussions, did have lots of conversations. Basically I understand how things going on in the background. Nate and Roman are great mentors, friends and the KDE is a great community.

Now, my todolist is as following:

**1.** Rewrite Touchpad KCM in QtQuick.</br>
**2.** Rewrite Mouse KCM in QtQuick.

Hope to see you next week! :)
<!------------------------FINALLY-->
