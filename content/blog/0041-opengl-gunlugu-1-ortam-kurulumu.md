---
title: "OpenGL Günlüğü 1 - Ortam Kurulumu, İlk Proje \"SA World!\""
date: "2019-09-07T12:49:09+03:00"
thumbnail: "/img/opengl-logo.jpg"
categories: ["Yazılar|Writings"]
tags: ["OpenGL", "C++", "C", "Başlangıç"]
url: "opengl-gunlugu-1-ortam-kurulumu"
summary: GPU'nun gücünden faydalanarak grafikleri kodlamak istiyorsanız doğru yerdesiniz. Bu yazıda, OpenGL ile ilk projenizi kodlayabilecek duruma gelene kadar yapılması gerekenleri adım adım yapacağız. OpenGL kütüphanesini kullanmak için tek yapmanız gereken
---

{{< goEnPost url="/opengl-on-linux-1-ide-setup" >}} <br>

GPU'nun gücünden faydalanarak grafikleri kodlamak istiyorsanız doğru yerdesiniz. Bu alanda pek tecrübeli olmayan kişiler OpenGL ile bu işe giriştiğinde, kafa karışıklığı oluşturması muhtemel bir çok farklı teknoloji (kütüphane, terim vb.) ile karşılaşacaktır. Bu yazıda, bu teknolojilere bir bakış atıp OpenGL ile ilk projenizi kodlayabilecek duruma gelene kadar yapılması gerekenleri adım adım yapacağız.

Bu yazı Ubuntu'yu temel alarak hazırlanmıştır fakat çoğu dağıtımda veya platformda takip edilmesi gereken adımlar aynı olacaktır. Bir sıkıntı olursa e-posta atabilirsiniz veya yorum yazabilirsiniz.

Proje dosyalarına şuradan erişebilirsiniz : [github.com/furkantokac/opengl-dev-qtcreator](https://github.com/furkantokac/opengl-dev-qtcreator)


### İçerik

**1.** İlgili Teknolojilere Bakış <br>
**2.** Bağımlılıkların Kurulumu <br>
**3.** Qt Creator Kurulumu <br>
**4.** İlk Proje, "SA World!" <br>
**5.** Proje Dosyalarına Bakış


### 1. İlgili Teknolojilere Bakış

##### Qt Creator

Verimli, güçlü olmasına rağmen çok hafif bir geliştirme ortamıdır (IDE). Temel olarak [Qt Framework](/qt-framework-genel-bakis)'ü için geliştirilmiştir fakat günümüzde farklı dilleri de desteklemektedir. Şahsi olarak C, C++ ile yazılmış büyük/küçük tüm projeler için işyerinde, evde, her yerde kullanmaktayım ve çok memnunum. Python projeleri için de kullanmayı planlamaktayım fakat ne kadar verimli olur şu an bilemiyorum.


##### CMake

İçerisinde bir sürü kaynak dosyası olan bir proje düşünün. (bkz. Linux çekirdeği). Bu projenin kaynak dosyalarının birbirleri arasındaki ilişkiyi takip etmek, bu ilişkiye göre doğru bir şekilde, doğru bir sıra ile kaynak dosyalarını derlemek büyük çaba gerektirir. CMake, bu işi kolaylaştırmak, otomatize etmek için geliştirilmiş bir projedir. 

Bir projenin kaynak dosyaları derleneceği zaman ***cmake*** programına, projenin ***CMakeLists.txt*** dosyası girdi (input) olarak verilir. ***CMakeLists.txt*** dosyasında projenin kaynak kodlarının birbirleriyle nasıl bağlantılı olduğu, hangi dosyaların proje dosyaları olduğu, bu dosyaların nerelerde tutulduğu, nasıl derlenmesi gerektiği vs. tanımlanır. Örnek olarak bizim projemizin [CMakeLists.txt](https://github.com/furkantokac/opengl-dev-qtcreator/blob/master/CMakeLists.txt) dosyasına ve GLFW projesinin [CMakeLists.txt](https://github.com/glfw/glfw/blob/b1309dd42a72c8f7cd58a6f75329c4328679aed2/CMakeLists.txt) dosyasına göz atabilirsiniz. ***cmake***, ***CMakeLists.txt*** çalıştırıldığında gerekli düzenlemeleri yaparak [***MakeFile***](https://stackoverflow.com/questions/25789644/difference-between-using-makefile-and-cmake-to-compile-the-code/25790020) dosyalarını oluşturur. Daha sonrasına ***make*** komutu girilir ve proje derlenir.

Diğer bir çok geliştirme ortamı gibi Qt Creator da CMake'i direkt olarak desteklemektedir. Ben CMake'i severek kullanırım, size de öneririm.


##### OpenGL

OpenGL, ekran kartını kontrol etmek amaçlı geliştirilmiş bir programlama arayüzüdür (API). Ben OpenGL'i standart olarak tanımlamayı seviyorum.

**[?]** *Neden bir standart gerekiyor ?* <br>
Bu standart, donanım ile uygulama arasında bir katmandır. Şöyle ki, donanımın anladığı yazılım dili, donanıma göre değişebilir (bkz. [video](https://www.youtube.com/watch?v=KHa-OSrZPGo)) ve içeride olanlar dışarıya tamamen açık değildir (ne yazık ki, çünkü özgür donanım/yazılım değil!). Bu katmanın üzerinde bir standart olması lazım ki yazılımcıların yazdığı yazılım, donanımlara bağımlı olmasın, aynı zamanda bu katman sayesinde ekran kartını daha kolaylıkla kontrol edebilsin. Herhalde kimse oyunlarda "Bu oyun sadece Nvidia 1070 ile çalışır." şeklinde ibare görmek istemez veya farklı ekran kartlarının farklı detaylarıyla uğraşmak istemez :) Bu nedenle standartlar oluşturulur (Vulkan, OpenGL, OpenCL, DirectX vs.), donanım üreticisi, ürettiği donanımın sürücüsüne bu standartı ekleyip destekler, böylelikle o standartı kullanarak yazılmış her program, o donanımda çalışır. Mesela Intel'in bazı grafik işleme donanımlarının (GPU) desteklediği standartları aşağıda görebilirsiniz.

[![](/img/intel-gpu-api-compatibilities.jpg)](https://www.intel.com/content/www/us/en/support/articles/000005524/graphics-drivers.html)

Eğer donanım, verimli bir şekilde bir standartı destekleyecekse, standart desteği sonradan da eklenebilir. Tabi nasıl ki donanım yeterli olmasına rağmen ticari kaygılar ile güncelleme almayan telefonlar varsa ekran kartlarında da ticari kaygılardan ötürü bu tarz durumlar yaşanabilir. Çözüm, özgür yazılım/donanım! Neyse konumuza dönelim. Mesela Intel yayınladığı sürücü ile GPU'larına Vulkan desteği verdiği zaman yapılan habere [şuradan](https://www.geeks3d.com/20180830/intel-hd-graphics-driver-v6286-released-vulkan-1-1-82-support-added/) ulaşabilirsiniz. Donanım değişmedi, sadece sürücüler ile gerekli destek sağlandı.

**[?]** *Yani bir donanım üzerinde OpenGL'i kullanabilmek için donanım üreticisinin OpenGL desteğine mi bağımlıyız ?* <br>
Bu konu çok detaylı bir konu. Yukarıdaki yazıda konuyu dağıtmadan temeli anlatmaya, en düz olan olarak neyin nasıl işlediğini anlatmaya çalıştık. Öyle ki sadece Windows için geliştirilmiş olan DirectX bile araya konulan katmanlar (abstraction) sayesinde Linux üzerinde çalışabilmektedir. (bkz.1 [Wine](https://www.winehq.org/), bkz.2 [Witcher 3 On Linux](https://www.youtube.com/watch?v=rusq83ETM9E)) Veya yukarıdaki Intel tablosunda gördüğünüz gibi HD 4000'in resmi olarak Vulkan desteği olmamasına rağmen, sahip olduğum 3630QM işlemcimin içindeki HD 4000'i Linux'ta *mesa-vulkan-drivers* sayesinde Vulkan destekli olarak kullanabiliyorum. ([Linux](https://youtu.be/oHNKTlz1lps) farkı 😎) Günün sonunda her şey 0 ve 1 olarak işleniyor, dolayısıyla birbirine dönüştürülebilir, yani her şey mümkün. Detaylara inmek isterseniz, derli toplu olarak bu konular ile ilgili bağlantıları, anahtar kelimeleri bir arada bulunduran, Nvidia için geliştirilmiş özgür ekran kartı sürücüsü projesi [Nouveau'nun "Get Involved" sayfasına](https://nouveau.freedesktop.org/wiki/IntroductoryCourse/) göz atabilirsiniz. İngilizce olarak anlamakta sıkıntı çekerseniz e-posta ile bana haber edebilirsiniz. Gerekirse çevirisini yaparız.


##### GLFW, GLAD, GLEW

**[?]** *GLFW nedir ?* <br>
Ekran kartı sürücüsünde donanım ile etkileşim halinde bulunan OpenGL API'si, dışarıdan direkt olarak kullanılabilir fakat OpenGL, bir çok temel özellikten yoksundur. Temel özellikten kasıt, mesela bir sistemde pencere oluşturmak, klavye, fare gibi girdileri yakalamak tamamen işletim sistemi ile alakalı bir iştir. Bu nedenle bu işler için her sisteme özel, farklı kod yazmak gerekir. GLFW bizim için bu kısımları, platform gözetmeksizin (cross-platform) hallediyor. Bir nevi OpenGL'e Assembly dersek GLFW C dili oluyor. Tam doğru bir benzetme olmasa da mantıksal olarak benziyor. GLFW'ye bir çok alternatif var. (bkz. FreeGlut, SDL) İstek/ihtiyaç olursa projemize diğer alternatifleri de ekleyebiliriz.

**[?]** *GLAD, GLEW nedir ?* <br>
Bunlar OpenGL eklentisi (extension) olarak geçiyor. OpenGL'in var olan fonksiyonlarına yapılan eklemeler diyebiliriz. Temel amacı farklı sistemler, OpenGL sürümleri, ekran kartı sürücüleri arasında oluşabilecek uyum sorunlarını çözmeye yarıyor. Detaylı bilgiye [buradan](https://www.khronos.org/opengl/wiki/OpenGL_Extension) ulaşabilirsiniz. GLAD mı GLEW mi kullanmalıyım derseniz, daha temiz olduğunu düşündüğümden dolayı GLAD derim. GLEW'i projeye dahil etmemizdeki amaç, internette bir çok örneğin GLEW kullanıyor olması. GLEW desteğinin projemize sonradan eklendiğini [buradan](https://github.com/furkantokac/opengl-dev-qtcreator/commits/master) görebilirsiniz. "Ben illa GLAD ile yoluma devam etmek istiyorum fakat bulduğum örnek GLEW kullanıyor" derseniz, örneğin GLEW kısımlarını kolaylıkla GLAD'a çevirebilirsiniz. Kurcalamaya çekinmeyin.


### 2. Bağımlılıkların Kurulumu

Aşağıda teknoloji ve bu teknolojinin bağımlılıklarını kurabilmek için gerekli komut verilmiştir.

**a.** GLFW :  `sudo apt-get install xorg-dev` <br>
**b.** GLEW : `sudo apt-get install build-essential libXmu-dev libXi-dev libgl-dev` <br>
**c.** CMake : `sudo apt-get install cmake` <br>
**d.** Git : `sudo apt-get install git`


### 3. Qt Creator Kurulumu

**a.** Qt Installer'ı [buradan](https://www.qt.io/download-qt-installer) indiriyoruz. <br>
**b.**&emsp;**->** İndirilen dosyaya sağ tıklıyoruz. <br>
&emsp;&emsp;**->** Özellikler (Properties) menüsüne gidiyoruz. <br>
&emsp;&emsp;**->** İzinler (Permissions) menüsüne gidiyoruz. <br>
&emsp;&emsp;**->** Çalıştırılabilir (isExecutable) olarak işaretliyoruz ve onaylıyoruz. <br>
**c.** Şimdi, indirilen dosyaya sol tıkladığımızda yükleyici otomatik olarak başlayacak. Eğer bir hareket olmazsa indirilen dosyayı terminal üzerinden çalıştırıp Log'ları, dolayısıyla sorunun kaynağını görebilirsiiz.  <br>
**d.** Yükleyicide ilerleyince karşımıza paket seçme ekranı çıkması lazım. İstediğiniz paketleri buradan seçebilirsiniz, Qt Creator her hâlükârda yüklenecektir. Benim paket seçimim aşağıdaki gibidir.

![ ](/img/qt-installation-packages.png#center)


### 4. İlk Proje, "SA World!"

**a.** Github üzerindeki dosyaları yinelemeli (recursive) olarak klonluyoruz (clone) :
<br>`git clone --recursive git@github.com:furkantokac/opengl-dev-qtcreator.git` <br>
**b.** Qt Creator'u açıyoruz.<br>
**c.** Proje Aç'a (Open Project) tıklıyoruz. <br>
**d.** Github'tan klonladığımız opengl-dev-creator klasörüne gidip CMakeLists.txt'u seçiyoruz. <br>
**e.** Qt Creator'da çalıştırtır kısmından OpenglDevTemplate'i seçiyoruz. (alttaki resimde gösteriliyor) <br>
**f.** Çalıştır'a basıyoruz. <br>
**g.** Her şey yolundaysa ekranınıza yanıp sönen bir pencere gelmesi lazım. Kütüphaneler ile birlikte gelen diğer örnekleri (alttaki resimde gösteriliyor) çalıştırarak inceleyebilirsiniz. Bir sıkıntı olması halinde e-posta veya yorum ile sıkıntıyı belirtebilirsiniz.

![ ](/img/qtcreator-run-glfw-examples.png#center)


### 5. Proje Dosyalarına Bakış

Temel olarak bizim çalışmalarımızı yapacağımız yer [src][srcFolder] klasörüdür. Yeni kaynak kod dosyalarını [src][srcFolder] klasörü altında oluşturursanız projeye otomatik olarak eklenecektir. Yeni dosyaların Qt Creator üzerinde gözükmesi için CMake'i yeniden çalıştırmanız gerek. Bunun için Qt Creator üzerinde projenin isminin üzerine sağ tıklayıp "CMake'i Çalıştır" (Run CMake)'e tıklayın. (alttaki resimde gösteriliyor) Yeni kaynak dosyası eklemek istemezseniz, tüm çalışmalarınızı, varsayılan olarak projeye ekli olan [src/main.cpp](https://github.com/furkantokac/opengl-dev-qtcreator/blob/master/src/main.cpp) üzerinde de yapabilirsiniz.


![ ](/img/qtcreator-run-cmake.png#center)

[srcFolder]: https://github.com/furkantokac/opengl-dev-qtcreator/tree/master/src
