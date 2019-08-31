---
title: "Windows'un yanına Linux (Ubuntu) Kurmak - Dualboot"
date: "2016-03-04T03:51:46+03:00"
thumbnail: "/img/ubuntu-windows-logo.png"
categories: ["Yazılarım"]
tags: ["yazı"]
url: "windowsun-yanina-linux-ubuntu-kurmak-dualboot"
---

<h3>Adım Adım :</h3>
Bu rehber Legacy Bios için geçerlidir.
<ol>
 	<li>Ubuntu'yu indiriyoruz. Stabilite için en son çıkan LTS sürümünü kurmanızı öneririm.
---<a href="http://www.ubuntu.com/download/desktop">İndirme Link</a></li>
 	<li>Ubuntu'yu USB'ye yazdırma
---<a href="http://www.ubuntu.com/download/desktop/create-a-usb-stick-on-windows">USBye Yazdır Link</a></li>
 	<li>Ubuntu için diskte bölüm oluşturma (videonun yalnızca ilk 42sn'si)
---<a href="https://www.youtube.com/watch?v=uGdrQxA0E6g">Bölüm Oluştur Link</a></li>
 	<li>(Eğer Windows8 kullanıyorsanız) Windows8'in fastboot özelliğini kapatma
---<a href="http://www.eightforums.com/attachments/tutorials/6232d1337576251-fast-startup-turn-off-windows-8-a-step-3.jpg">Fast Boot Kapat Link</a></li>
 	<li>BIOStan Secure Boot modunu kapatma (Anakarta göre bios arayüzleri değişiyor o yüzden sizde farklı olabilir veya bu ayar olmayada bilir)
---<a href="http://www.top-password.com/blog/wp-content/uploads/2013/01/disable-secure-boot.jpg">Secure Boot Kapat Link</a></li>
 	<li>Ubuntu'yu kopyaladığımız USByi bilgisayara takarak tekrar BIOSa girip, boot sekmesinin altından first boot olarak USByi seçiyoruz ki sistem USBden başlasın.
---<a href="http://www.wimware.com/design/how-to/boot-from-usb/boot-usb4.jpg">First Boot Ayarla Link</a></li>
 	<li>Exit sekmesinden Exit and save diyip bekliyoruz. Şuan sistemin USBden, yani Ubuntu'dan başlaması gerek.</li>
 	<li>Ubuntu'yu boot edip kurma videosu. Boot ekranı sizde farklı olabilir.
---<a href="https://youtu.be/uGdrQxA0E6g">Ubuntu Kurulum Link</a></li>
 	<li>Swap alanını RAMinizin miktarı kadar yapın. Mesela 4gb raminiz varsa 4096mb yapın. Gerisini videoda anlattığı gibi yapabilirsiniz.</li>
 	<li>Yükleme bitince USB'yi çıkarıp Sistemi Ubuntu'dan başlatabilirsiniz.</li>
</ol>
<h3>Ubuntu Kullanımı (İlk defa Ubuntu kullanacaklar için) :</h3>
<ol>
 	<li>Ubuntu kullanı hakkında genel bilgi :<b> <a href="https://youtu.be/PTaL1s3YJPY">https://youtu.be/PTaL1s3YJPY</a></b></li>
 	<li>Komut sistemine alıştırma : <b><a href="https://youtu.be/vhZLTp6N4XA">https://youtu.be/vhZLTp6N4XA</a></b></li>
 	<li>Linux kullanımı ile ilgili genel bilgi edinip sistemin yapısını öğrenmek için kesinlikle okumanız gereken bir kaynak(Ufak tefek çalışmayan şeyler olabilir mesela ilk başta gösterdiği "mail" komutu sisteme direkt yüklü değil onu geçin diğer her şeyi olabildiğince deneyin mantığını anlayın)
<div class="western"><b>Kim Korkar Unixten <a href="https://dl.dropboxusercontent.com/u/27587858/kim.korkar.unixden.pdf?dl=1">https://dl.dropboxusercontent.com/u/27587858/kim.korkar.unixden.pdf?dl=1</a></b></div>
&nbsp;</li>
</ol>
