---
title: "This Week in KDE, Part 3 : Touchpad KCM, Mouse KCM, Libinput"
date: "2018-06-03T04:26:56+03:00"
thumbnail: "/img/kdeplasma-3.jpg"
categories: ["KDE"]
tags: ["English", "KDE", "FOSS Dev"]
url: "this-week-in-kde-part-3"
---

### Cheap talk

The previous days were full with discussions since the changes I'm working on will affect the user experience directly. At last, we decided to ship the changes as it is and apply the other changes part by part. I did a Touchpad [KCM](https://techbase.kde.org/Development/Tutorials/KCM_HowTo) UI redesign in [Kirigami](https://www.kde.org/products/kirigami/) and I would do it for Mouse KCM too but Mouse KCM had some name changes last time so small problem occurred in my system. (KCM's are part of the KDE Plasma Desktop so changes may be affected by other things.) The code is ready. After solving the problem, new Mouse KCM will be shipped :) My first coding phase plan of GSoC is kind of finished, too. My next task will be about Libinput so currently, I'm working on [a bug](https://bugs.freedesktop.org/show_bug.cgi?id=106340) to get used to Libinput hacking and to introduce myself to the Libinput community (actually better to say Wayland community).

**Old Touchpad KCM**
![](https://phabricator.kde.org/file/data/v5q2ugniivxaco3t7j7y/PHID-FILE-5smfihd23tgesucnn335/image.png)

**New Touchpad KCM**
![](https://phabricator.kde.org/file/data/eygmifg26ze6m7pkw6r7/PHID-FILE-m4hjo763mal3uzwxuu6l/image.png)
<br>
<br>
<!----------------------CHEAP TALK-->


### Show me the code

**1.** Redesign Touchpad KCM in Kirigami. Commit is [here](https://commits.kde.org/plasma-desktop/dd1244d6676620c06011b6c1db0c0ff3d5cdf0ab). Phab is [here](https://phabricator.kde.org/D13141). </br>
**2.** We worked on the [Bug 387156](https://bugs.kde.org/show_bug.cgi?id=387156) but the patch will come later after more discussions.
<!---------------SHOW ME THE CODE-->


### Finally

My todolist is as following:

**1.** Publish Mouse KCM redesign. </br>
**2.** Fix the Libinput bug.

Hope to see you next week! :)
<!------------------------FINALLY-->
