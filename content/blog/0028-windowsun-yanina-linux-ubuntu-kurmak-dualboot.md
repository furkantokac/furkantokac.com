---
title: "Windows'un yanına Linux (Ubuntu) Kurmak - Dualboot"
date: "2016-03-04T03:51:46+03:00"
lastmod: "2019-09-02T03:14:09+03:00"
thumbnail: "/img/ubuntu-windows-logo.png"
categories: ["Yazılar|Posts"]
tags: ["Yazı", "Linux"]
url: "windowsun-yanina-linux-ubuntu-kurmak-dualboot"
---

### Adım Adım :

Bu rehber Legacy Bios için geçerlidir.

1. Ubuntu'yu indiriyoruz. Stabilite için en son çıkan LTS sürümünü kurmanızı öneririm. [İndirme Link](http://www.ubuntu.com/download/desktop)
1. Ubuntu'yu USB'ye yazdırma [USBye Yazdır Link](http://www.ubuntu.com/download/desktop/create-a-usb-stick-on-windows)
1. Ubuntu için diskte bölüm oluşturma (videonun yalnızca ilk 42sn'si) [Bölüm Oluştur Link](https://www.youtube.com/watch?v=uGdrQxA0E6g)
1. (Eğer Windows8 kullanıyorsanız) Windows8'in fastboot özelliğini kapatma [Fast Boot Kapat Link](http://www.eightforums.com/attachments/tutorials/6232d1337576251-fast-startup-turn-off-windows-8-a-step-3.jpg)
1. BIOStan Secure Boot modunu kapatma (Anakarta göre bios arayüzleri değişiyor o yüzden sizde farklı olabilir veya bu ayar olmayada bilir) [Secure Boot Kapat Link](http://www.top-password.com/blog/wp-content/uploads/2013/01/disable-secure-boot.jpg)
1. Ubuntu'yu kopyaladığımız USByi bilgisayara takarak tekrar BIOSa girip, boot sekmesinin altından first boot olarak USByi seçiyoruz ki sistem USBden başlasın. [First Boot Ayarla Link](http://www.wimware.com/design/how-to/boot-from-usb/boot-usb4.jpg)
1. Exit sekmesinden Exit and save diyip bekliyoruz. Şuan sistemin USBden, yani Ubuntu'dan başlaması gerek.
1. Ubuntu'yu boot edip kurma videosu. Boot ekranı sizde farklı olabilir. [Ubuntu Kurulum Link](https://youtu.be/uGdrQxA0E6g)
1. Swap alanını RAMinizin miktarı kadar yapın. Mesela 4gb raminiz varsa 4096mb yapın. Gerisini videoda anlattığı gibi yapabilirsiniz.
1. Yükleme bitince USB'yi çıkarıp Sistemi Ubuntu'dan başlatabilirsiniz.


### Ubuntu Kullanımı (İlk defa Ubuntu kullanacaklar için) :

1. Ubuntu kullanı hakkında genel bilgi : [https://youtu.be/PTaL1s3YJPY](https://youtu.be/PTaL1s3YJPY)
1. Komut sistemine alıştırma : [https://youtu.be/vhZLTp6N4XA](https://youtu.be/vhZLTp6N4XA)
1. Linux kullanımı ile ilgili genel bilgi edinip sistemin yapısını öğrenmek için kesinlikle okumanız gereken bir kaynak (Ufak tefek çalışmayan şeyler olabilir mesela ilk başta gösterdiği "mail" komutu sisteme direkt yüklü değil onu geçin diğer her şeyi olabildiğince deneyin mantığını anlayın) : [Kim Korkar Unixten][kimkorkarunixten]

[kimkorkarunixten]: http://cayfer.bilkent.edu.tr/~cayfer/kku/kku.html
