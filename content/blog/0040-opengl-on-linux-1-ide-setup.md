---
title: "OpenGL On Linux 1 - IDE Setup : Qt Creator, CMake, GLFW, X11, Ubuntu"
date: "2019-08-23T23:38:09+03:00"
lastmod: "2019-08-30T23:38:09+03:00"
thumbnail: "/img/opengl-logo.jpg"
categories: ["OpenGL"]
tags: ["opengl", "english", "cpp", "c"]
url: "opengl-on-linux-1-ide-setup"
---

If you want to start OpenGL development by using Qt Creator, you're on the right place. By this post, you'll have a ready-to-work development environment for OpenGL development. The OpenGL libraries will be automatically in your "libs" folder so no need to worry about importing them. Just #include &lt;libraryYouWant.h>. I'll not mention about each technologies seperately (like what is CMake etc.) so you can search them online if you want.

Although the post mainly targets to Ubuntu, it should work for the most of the distros and platforms as long as you have the dependencies. If you're not using Ubuntu, I think you already know how to survive :) If you have any problem, just comment.

You can find the project on my GitHub account : <a href="https://github.com/furkantokac/opengl-dev-qtcreator">github.com/furkantokac/opengl-dev-qtcreator</a>

**Currently supported libraries** : GLFW, GLEW, GLAD

### Outline
**1.** Installing Dependencies, CMake, Git
</br>**2.** Installing Qt Creator
</br>**3.** Clone The Template And Run On Qt Creator
</br>**4.** How To Use The Template

### 1. Installing Dependencies, CMake, Git
**a.** GLFW Deps : `sudo apt-get install xorg-dev`
</br>**b.** GLEW Deps : `sudo apt-get install build-essential libXmu-dev libXi-dev libgl-dev`
</br>**c.** CMake : `sudo apt-get install cmake`
</br>**d.** Git : `sudo apt-get install git`

### 2. Installing Qt Creator
**a.** Download Qt Installer from <a href="https://www.qt.io/download-qt-installer">here, Qt's website</a>.
</br>**b.** Right click to the downloaded file **->** Properties **->** Permissions **->** Mark as executable
</br>**c.** Click the file and it should run. (If nothing happens, run it from the terminal to see the problem.)
</br>**d.** Select the packages you want. Qt Creator will be installed in anyway. My installation is as the following;

![ ](/img/qt-installation-packages.png#center)

### 3. Clone The Template And Run On Qt Creator
**a.** Clone (recursively) the template that I prepare by the following command : </br>`git clone --recursive git@github.com:furkantokac/opengl-dev-qtcreator.git`
</br>**b.** Go to Qt Creator **->** Open Project **->** choose the CMakeLists.txt under opengl-dev-creator directory **->** Choose OpenglDevTemplate to run (check the following image) **->** Run **->** Enjoy
</br>**c.** If you see a flickering window, you're done. You can also try other examples (check the following image) If you're in trouble, you can comment the problem.

![ ](/img/qtcreator-run-glfw-examples.png#center)

### 4. How To Use The Template
You can work on src folder. New files/folders under src folder will be automatically added to the project after you re-run the CMake. To re-run CMake, right click the project name on Qt Creator and click "Run CMake". (see the following image) As default, src/main.cpp is running so you can directly edit main.cpp if you don't want to add new files.

![ ](/img/qtcreator-run-cmake.png#center)
