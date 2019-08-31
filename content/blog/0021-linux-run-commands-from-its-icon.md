---
title: "Linux - Run Commands From It's Icon"
date: "2014-08-26T07:37:40+03:00"
thumbnail: "/img/linux-orange-gradient.jpg"
categories: ["Yazılarım"]
tags: ["linux", "english", "yazı"]
url: "linux-run-commands-from-its-icon"
---

<a href="http://furkantokac.com/?p=165"><span style="font-size: 15px;">(Bu yazının Türkçesi için tıklayın)</span></a>
<h3>Description :</h3>
Thanks to this file, we can start our programs without terminal even if it needs to get command on terminal to start (like Python programs) with double click to its icon (really .png icon :) ) or we can write commands to facilitate our works.
Our examples will be thats :
-I have been writing FTassembler (will be published soon in my blog :) ) program using Python and Tkinter (GUI). Firstly, we run this program just by double click without TERMINAL.
-After that, I share informations to copy a file <span id="result_box" class="short_text" lang="en"><span class="hps">from place to</span> <span class="hps">place by double click without terminal.</span></span>
<h3>Running A Python Script By Double Click Without Terminal :</h3>
<ol>
 	<li>Open text editor.</li>
 	<li> Copy the <span id="result_box" class="short_text" lang="en"><span class="hps">following text to text editor. </span></span>(Also there is mine and a screenshot below as example)
<pre>[Desktop Entry]
Version=1.0
Encoding=UTF-8
Type=Type of our process
Terminal=Choose whether to open the terminal 
Name=Name of double click file
Exec=Command(This command will run by terminal)
Comment=Comment about process
Icon=Icon's location
Name[en]=English name of double click file</pre>
My text :
<pre>[Desktop Entry]
Version=1.0
Encoding=UTF-8
Type=Application
Terminal=false
Name=KomutPython
Exec=python3 /home/ft/CiftTiklama/FTassembler.py
Comment=Assembler
Icon=/home/ft/CiftTiklama/IconPython.png
Name[en]=KomutPython
</pre>
Screenshot :
<a href="http://furkantokac.com/wp-content/uploads/2016/02/Gosterim1.png"><img class="aligncenter wp-image-167 size-full" src="http://furkantokac.com/wp-content/uploads/2016/02/Gosterim1.png" width="1600" height="900" /></a></li>
 	<li>After that, save your file as fileName.desktop to wherever you want.</li>
 	<li>Finally, we need to set permission issue of fileName.desktop :
Use command : <span style="background-color: #cfe2f3;"><span style="background-color: #cfe2f3;"><code>chmod u=rwxst fileName.desktop
</code></span></span>
Or use GUI :
a) Right click to fileName.desktop.
b) Continue with "Properties".
c) Go to "Permissions" tab and activate the "Execute".</li>
 	<li>After all these operations, .desktop part of fileName will be invisible and <span id="result_box" class="" lang="en"><span class="hps">you</span> <span class="hps">will be able to</span> <span class="hps">run the</span> <span class="hps">program w</span><span class="hps">herever you want</span><span class="">.</span></span></li>
</ol>
<h3>Screenshot after the bouble click to my command file : (No terminal)</h3>
<a href="http://furkantokac.com/wp-content/uploads/2016/02/Goster2ENG.png"><img class="aligncenter wp-image-173 size-full" src="http://furkantokac.com/wp-content/uploads/2016/02/Goster2ENG.png" width="1083" height="692" /></a>

<span style="font-size: 14px; line-height: 19.6px; text-align: justify; font-family: 'Helvetica Neue', Arial, Helvetica, sans-serif;"><span style="font-size: small;"> </span></span>

As you know, <span id="result_box" class="" lang="en"><span class="hps">this</span> <span class="hps">is not</span> <span class="hps">only</span> <span class="hps">to run</span> <span class="hps">our</span> <span class="hps">Python programs</span>. This <span class="hps">can also be used for</span><span class="hps">many different processes</span>. </span>For example, to copy a file by double click, your fileName.desktop should be like this :
<pre>[Desktop Entry]
Version=1.0
Encoding=UTF-8
Type=Application
Terminal=false
Exec=cp /home/ft/Desktop/Target.png /home/ft/Desktop/CopyOfTarget.png
Comment=Copy a file
Icon=/home/ft/Desktop/CopyIcon.png
Name=CopyCommand 
Name[en]=CopyCommand
</pre>
Name of this process is Desktop Entry. You can find more info about Desktop Entry from Google.
My OS : Ubuntu 14.04
