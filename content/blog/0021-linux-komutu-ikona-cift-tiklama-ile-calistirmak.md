---
title: "Linux - Komutu İkona Çift Tıklama İle Çalıştırmak"
date: "2014-08-26T07:37:33+03:00"
thumbnail: "/img/linux-orange-gradient.jpg"
categories: ["Yazılar|Posts"]
tags: ["Yazı", "Linux"]
url: "linux-komutu-ikona-cift-tiklama-ile-calistirmak"
summary: "Bu işlem sayesinde ister çalıştırılabilir (executable) dosya oluşturmayan (Python gibi), terminal aracılığı ile komut girerek çalıştırılan programlarınızı çift tıklama ile çalıştırabilirsiniz, isterseniz de kendinize ait küçük komutlar yazabilirsiniz(.bat dosyaları gibi)."
---

{{< goEnPost url="/linux-run-commands-from-its-icon" >}} <br>

### Açıklama :

Bu işlem sayesinde ister çalıştırılabilir (executable) dosya oluşturmayan (Python gibi), terminal aracılığı ile komut girerek çalıştırılan programlarınızı çift tıklama ile çalıştırabilirsiniz, isterseniz de kendinize ait küçük komutlar yazabilirsiniz(.bat dosyaları gibi).

Şimdi yapacağımız örnek :

- Python'da Tkinter (GUI) kütüphanesi ile yazmakta olduğum FTassembler (yakında bloğumda yayınlayacağım :) ) programını çift tıklama ile, terminale girmeden çalıştıracağız.
- En sonda ise bir belgeyi başka bir yere kopyalamak için gerekli bilgileri vereceğim.


### Python GUI Projemizi İkon İle Terminale Girmeden Çalıştırmak

**1.** Metin Editör'ümüzü (text editor) açıyoruz. <br>
**2.** Aşağıdaki bilgileri belgemize kopyalıyoruz.(Aşağısında örnek olarak benim kullandığım belgenin bilgileri ve belgenin ekran görüntüsü mevcut)

{{< highlight bash >}}
[Desktop Entry]
Version=1.0
Encoding=UTF-8
Type=Çalıştırılacak komuta göre ayarlıyoruz
Terminal=Terminalin açılıp açılmayacağını seçiyoruz
Exec=Komutumuzu buraya yazacağız
Comment=Dosyamızın yapacağı işin kısaca tanımı
Icon=Çift tıklanacak dosyamızın ikonu
Name=Çift tıklanacak dosyamızın adı 
Name[en]=Çift tıklanacak dosyamızın İngilizce adı
{{< /highlight >}}

Benim bilgilerim :

{{< highlight bash >}}
[Desktop Entry]
Version=1.0
Encoding=UTF-8
Type=Application
Terminal=false
Exec=python3 /home/ft/CiftTiklama/FTassembler.py
Comment=Assembler
Icon=/home/ft/CiftTiklama/IconPython.pngName=KomutPython
Name[en]=KomutPython
{{< /highlight >}}

Belgemin ekran görüntüsü : <br>
[![](/img/desktop-entry-ss-1.jpg)](/img/desktop-entry-ss-1.png)

**3.** Daha sonra belgemizi belgeAdi.desktop yazarak istediğiniz yere kaydediyoruz.</li>
**4.** Son olarak belgeAdi.desktop dosyamıza gereken izini (Executable olması için) vereceğiz. <br>
&emsp; **Yöntem 1.** Teriminale şu komutu yazın : `chmod u=rwxst belgeAdi.desktop` <br>
&emsp; **Yöntem 2.** Veyahut manual olarak izin verebilirsiniz : <br>
&emsp;&emsp; **a.** belgeAdi.desktop belgemize sağ tıklıyoruz. <br>
&emsp;&emsp; **b.** Özellikler(Properties)'e basıyoruz. <br>
&emsp;&emsp; **c.** İzinler (Permissions) sekmesine giriyoruz ve en alttaki Çalıştırma (Execute) seçeneğimizi aktif ediyoruz.

**5.** Bu işlemden sonra belgenizin .desktop kısmı görünmez olacak ve programınızı istediğiniz yerden çalıştırabileceksiniz.

KomutPython'a çift tıkladıktan sonraki ekran görüntüsü : <br>
[![](/img/desktop-entry-ss-2-tr.png)](/img/desktop-entry-ss-2-tr.png)

Tabiki bu işlemi sadece Python programlarınızı çalıştırmak için değil çok farklı işlemler için de kullanabilirsiniz. Örneğin bir belgeyi terminale girmeden çift tıklama ile başka bir yere kopyalamak için belgeAdi.desktop 'a aşağıdaki bilgileri giriyoruz :

{{< highlight bash >}}
[Desktop Entry]
Version=1.0
Encoding=UTF-8
Type=Application
Terminal=false
Exec=cp /home/ft/Desktop/Hedef.png /home/ft/Desktop/Kopyasi.png
Comment=Assembler
Icon=/home/ft/Desktop/Kopyala.png
Name=Komutum 
Name[en]=Komutum
{{< /highlight >}}

Aslında burada ***Desktop Entry*** oluşturuyoruz. Konu hakkında detaylı bilgi için ***Desktop Entry*** anahtar kelimesini kullanarak arama yapabilirsiniz.
