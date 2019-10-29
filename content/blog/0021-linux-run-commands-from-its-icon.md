---
title: "Linux - Run Commands From It's Icon"
date: "2014-08-26T07:37:40+03:00"
thumbnail: "/img/linux-orange-gradient.jpg"
categories: ["Yazılar|Writings"]
tags: ["Linux", "English", "Writing"]
url: "linux-run-commands-from-its-icon"
summary: "Thanks to this file, we can start our programs without terminal even if it needs to get command on terminal to start (like Python programs) with double click to its icon (really .png icon :) ) or we can write commands to facilitate our works."
---

{{< goTrPost url="/linux-komutu-ikona-cift-tiklama-ile-calistirmak" >}} <br>

### Description :

Thanks to this file, we can start our programs without terminal even if it needs to get command on terminal to start (like Python programs) with double click to its icon (really .png icon :) ) or we can write commands to facilitate our works.

Our examples will be thats :

- I have been writing FTassembler (will be published soon in my blog :) ) program using Python and Tkinter (GUI). Firstly, we run this program just by double click without TERMINAL.
- After that, I share informations to copy a file from place to place by double click without terminal.


### Run Python GUI Program By Icon Without Terminal

**1.** Open text editor. <br>
**2.** Copy the following text to text editor. (Also there is mine and a screenshot below as example)

{{< highlight bash >}}
[Desktop Entry]
Version=1.0
Encoding=UTF-8
Type=Type of our process
Terminal=Choose whether to open the terminal 
Name=Name of double click file
Exec=Command(This command will run by terminal)
Comment=Comment about process
Icon=Icon's location
Name[en]=English name of double click file
{{< /highlight >}}

My text :

{{< highlight bash >}}
[Desktop Entry]
Version=1.0
Encoding=UTF-8
Type=Application
Terminal=false
Name=KomutPython
Exec=python3 /home/ft/CiftTiklama/FTassembler.py
Comment=Assembler
Icon=/home/ft/CiftTiklama/IconPython.png
Name[en]=KomutPython
{{< /highlight >}}

Screenshot : <br>
[![](/img/desktop-entry-ss-1.jpg)](/img/desktop-entry-ss-1.png)

**3.** After that, save your file as fileName.desktop to wherever you want. <br>
**4.** Finally, we need to set permission issue of fileName.desktop : <br>
&emsp; **Method 1.** use command : `chmod u=rwxst fileName.desktop` <br>
&emsp; **Method 2.** use GUI : <br>
&emsp;&emsp; **a.** Right click to fileName.desktop. <br>
&emsp;&emsp; **b.** Continue with "Properties". <br>
&emsp;&emsp; **c.** Go to "Permissions" tab and activate the "Execute".

**5.** After all these operations, .desktop part of fileName will be invisible and you will be able to run the program wherever you want.

Screenshot after the double click to my command file : (No terminal) <br>
[![](/img/desktop-entry-ss-2-en.png)](/img/desktop-entry-ss-2-en.png)

As you know, this is not only to run our Python programs. This can also be used for many different processes. For example, to copy a file by double click, your fileName.desktop should be like this :

{{< highlight bash >}}
[Desktop Entry]
Version=1.0
Encoding=UTF-8
Type=Application
Terminal=false
Exec=cp /home/ft/Desktop/Target.png /home/ft/Desktop/CopyOfTarget.png
Comment=Copy a file
Icon=/home/ft/Desktop/CopyIcon.png
Name=CopyCommand 
Name[en]=CopyCommand
{{< /highlight >}}

Name of this process is ***Desktop Entry***. You can find more info about ***Desktop Entry*** from Google.
