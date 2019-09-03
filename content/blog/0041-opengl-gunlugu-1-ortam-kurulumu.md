---
title: "OpenGL Günlüğü 1 - Ortam Kurulumu, İlk Proje \"SA World!\""
date: "2019-09-03T01:54:09+03:00"
thumbnail: "/img/opengl-logo.jpg"
categories: ["Yazılar|Posts"]
tags: ["OpenGL", "C++", "C", "Başlangıç"]
url: "opengl-gunlugu-1-ortam-kurulumu"
publishdate: 2023-09-03
---

GPU'nun gücünden faydalanarak grafikleri kodlamak istiyorsanız doğru yerdesiniz. Bu alanda pek tecrübeli olmayan kişiler OpenGL ile bu işe giriştiğinde, kafa karışıklığı oluşturması muhtemel bir çok farklı teknoloji (kütüphane, terim vb.) ile karşılaşacaktır. Bu yazıda, bu teknolojilere bir bakış atıp OpenGL ile ilk projenizi kodlayabilecek duruma gelene kadar yapılması gerekenleri adım adım anlatacağız.

Bu yazı Ubuntu'yu temel alarak hazırlanmıştır fakat çoğu dağıtımda veya platformda takip edilmesi gereken adımlar aynı olacaktır. Bir sıkıntı olursa e-posta atabilirsiniz veya yorum yazabilirsiniz.

Proje dosyalarına şuradan erişebilirsiniz : [github.com/furkantokac/opengl-dev-qtcreator](https://github.com/furkantokac/opengl-dev-qtcreator)


### İçerik

**1.** İlgili Teknolojilere Bakış <br>
**2.** Bağımlılıkların Kurulumu <br>
**3.** Qt Creator Kurulumu <br>
**4.** İlk Proje, "SA World!" <br>
**5.** Proje Dosyalarına Bakış


### 1. İlgili Teknolojilere Bakış

Yazı.


### 2. Bağımlılıkların Kurulumu

**a.** GLFW Deps : `sudo apt-get install xorg-dev` <br>
**b.** GLEW Deps : `sudo apt-get install build-essential libXmu-dev libXi-dev libgl-dev` <br>
**c.** CMake : `sudo apt-get install cmake` <br>
**d.** Git : `sudo apt-get install git`


### 3. Qt Creator Kurulumu

**a.** Download Qt Installer from <a href="https://www.qt.io/download-qt-installer">here, Qt's website</a>. <br>
**b.**&emsp;**->** Right click to the downloaded file <br>
&emsp;&emsp;**->** Properties <br>
&emsp;&emsp;**->** Permissions <br>
&emsp;&emsp;**->** Mark as executable <br>
**c.** Click the file and it should run. (If nothing happens, run it from the terminal to see the problem.) <br>
**d.** Select the packages you want. Qt Creator will be installed in anyway. My installation is as the following;

![ ](/img/qt-installation-packages.png#center)


### 4. İlk Proje, "SA World!"

**a.** Clone (recursively) the template that I prepare by the following command : <br>`git clone --recursive git@github.com:furkantokac/opengl-dev-qtcreator.git` <br>
**b.**&emsp;**->** Go to Qt Creator <br>
&emsp;&emsp;**->** Open Project <br>
&emsp;&emsp;**->** Choose the CMakeLists.txt under opengl-dev-creator directory <br>
&emsp;&emsp;**->** Choose OpenglDevTemplate to run (check the following image) <br>
&emsp;&emsp;**->** Run & Enjoy. <br>
**c.** If you see a flickering window, you're done. You can also try other examples (check the following image) If you're in trouble, you can comment the problem.

![ ](/img/qtcreator-run-glfw-examples.png#center)


### 5. Proje Dosyalarına Bakış

You can work on src folder. New files/folders under src folder will be automatically added to the project after you re-run the CMake. To re-run CMake, right click the project name on Qt Creator and click "Run CMake". (see the following image) As default, src/main.cpp is running so you can directly edit main.cpp if you don't want to add new files.

![ ](/img/qtcreator-run-cmake.png#center)
