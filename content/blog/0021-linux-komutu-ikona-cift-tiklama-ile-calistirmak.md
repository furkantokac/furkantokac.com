---
title: "Linux - Komutu İkona Çift Tıklama İle Çalıştırmak"
date: "2014-08-26T07:37:33+03:00"
thumbnail: "/img/linux-orange-gradient.jpg"
categories: ["Yazılar|Posts"]
tags: ["Yazı", "Linux"]
url: "linux-komutu-ikona-cift-tiklama-ile-calistirmak"
---

Açıklama :

Bu işlem sayesinde ister çalıştırılabilir (executable) dosya oluşturmayan (Python gibi), terminal aracılığı ile komut girerek çalıştırılan programlarınızı çift tıklama ile çalıştırabilirsiniz, isterseniz de kendinize ait küçük komutlar yazabilirsiniz(.bat dosyaları gibi).

Şimdi yapacağımız örnek :
-Python'da Tkinter (GUI) kütüphanesi ile yazmakta olduğum FTassembler (yakında bloğumda yayınlayacağım :) ) programını çift tıklama ile, terminale girmeden çalıştıracağız.
-En sonda ise bir belgeyi başka bir yere kopyalamak için gerekli bilgileri vereceğim.
<h3>Python Projemizi Çift Tıklama İle Terminale Girmeden Çalıştırmak :</h3>
<ol>
 	<li>Metin Editör'ümüzü (text editor) açıyoruz.</li>
 	<li>Aşağıdaki bilgileri belgemize kopyalıyoruz.(Aşağısında örnek olarak benim kullandığım belgenin bilgileri ve belgenin ekran görüntüsü mevcut)
<pre>[Desktop Entry]
Version=1.0
Encoding=UTF-8
Type=Çalıştırılacak komuta göre ayarlıyoruz
Terminal=Terminalin açılıp açılmayacağını seçiyoruz
Exec=Komutumuzu buraya yazacağız
Comment=Dosyamızın yapacağı işin kısaca tanımı
Icon=Çift tıklanacak dosyamızın ikonu
Name=Çift tıklanacak dosyamızın adı 
Name[en]=Çift tıklanacak dosyamızın İngilizce adı</pre>
Benim bilgilerim :
<pre>[Desktop Entry]
Version=1.0
Encoding=UTF-8
Type=Application
Terminal=false
Exec=python3 /home/ft/CiftTiklama/FTassembler.py
Comment=Assembler
Icon=/home/ft/CiftTiklama/IconPython.pngName=KomutPython
Name[en]=KomutPython
</pre>
Belgemin ekran görüntüsü :
<img class="aligncenter size-full wp-image-167" src="http://furkantokac.com/wp-content/uploads/2016/02/Gosterim1.png" alt="" width="1600" height="900" /></li>
 	<li>Daha sonra belgemizi belgeAdi.desktop yazarak istediğiniz yere kaydediyoruz.</li>
 	<li>Son olarak belgeAdi.desktop dosyamıza gereken izini (Executable olması için) vereceğiz.
Komut ile izin vermek için teriminale şu komutu yazın : <span style="background-color: #cfe2f3;"><span style="background-color: #cfe2f3;"><code>chmod u=rwxst belgeAdi.desktop
</code></span></span>
Veyahut manual olarak izin verebilirsiniz :
a) belgeAdi.desktop belgemize sağ tıklıyoruz.
b) Özellikler(Properties)'e basıyoruz.
c) İzinler (Permissions) sekmesine giriyoruz ve en alttaki Çalıştırma (Execute) seçeneğimizi aktif ediyoruz.</li>
 	<li>Bu işlemden sonra belgenizin .desktop kısmı görünmez olacak ve programınızı istediğiniz yerden çalıştırabileceksiniz.</li>
</ol>
<h3>KomutPython'a çift tıkladıktan sonraki ekran görüntüsü :</h3>
<img class="aligncenter size-full wp-image-168" src="http://furkantokac.com/wp-content/uploads/2016/02/Goster2TR.png" alt="" width="1083" height="692" />

<span style="font-size: 14px; line-height: 19.6px; text-align: justify; font-family: 'Helvetica Neue', Arial, Helvetica, sans-serif;"><span style="font-size: small;"> </span></span>

Tabiki bu işlemi sadece Python programlarınızı çalıştırmak için değil çok farklı işlemler için de kullanabilirsiniz. Örneğin bir belgeyi terminale girmeden çift tıklama ile başka bir yere kopyalamak için belgeAdi.desktop 'a aşağıdaki bilgileri giriyoruz :
<pre>[Desktop Entry]
Version=1.0
Encoding=UTF-8
Type=Application
Terminal=false
Exec=cp /home/ft/Desktop/Hedef.png /home/ft/Desktop/Kopyasi.png
Comment=Assembler
Icon=/home/ft/Desktop/Kopyala.png
Name=Komutum 
Name[en]=Komutum
</pre>
