---
title: "C C++ Java Dillerinde 8lik 16lık Sayı Sistemlerini Kullanmak"
date: "2013-08-21T23:05:58+03:00"
thumbnail: "/img/matrix-like-binary.jpg"
categories: ["Yazılar|Writings"]
tags: ["Yazı"]
url: "c-c-java-dillerinde-8lik-16lik-sayi-sistemlerini-kullanmak"
---

### Açıklama :
Programlarımızda 10'luk tabanı kullanmamıza rağmen, bazen 8'lik(octal) ve 16'lık(hexadecimal yani kısaca hex) tabanları da kullanmamız gerekebiliyor.
Buna istinaden C/C++/Java bize şöyle kolaylık sağlıyor:

* OCTAL(8'lik sayı sistemi) sayı yazmak için, sayımızın başına 0 ekleyerek, bunun octal olduğunu belirtebiliyoruz.
Örnek : 032 = 32(8) = 26(10)

* HEX(16'lık sayı sistemi) sayı yazmak için, sayımızın başına 0x veya 0X ekleyerek, bunun hex olduğunu belirtebiliyoruz.
Örnek : 0x23 = 23(16) = 35(10)

* Negatif değerleri ise direkt -0x23 şeklinde yazabiliriz.

### Örnek :
Şöyle bir ifade yazdığımızı varsayalım: `int x = 0x23;`

Derleyici bunu, int tipte değişkene hex bir veri girildi diye not etmez.
Bildiğiniz gibi bilgisayarlar, verileri 2'lik(binary) sisteme göre depolar.

Şimdi bu ifadeyi yazdığımızı varsayalım: `int x = 29;`

Bu ifade, hafızadaki konuma 2 ve 9 şeklinde depolanmaz. Bunun yerine, bilgisayar sayıyı binary'ye çevirir.
Direkt olarak 10'luk sistemde yazmamız, programlama dilini yazan kişilerin, dili kullananlara sağladığı bir kolaylıktır.
Bilgisayarda yazdığımız herşeyin, bir şekilde binary sisteme çevrildiğini bilmemiz gerekir.

Bir değişkene 0x23 veya 35 atamanız, bilgisayar için aynı şeydir. İkisi de binary sisteme göre kaydedilir. Sizin 0x23 veya 35 yazmış olmanızın bir önemi yoktur. Bu yalnız derleyici için fark eder. Derleyici, duruma göre hex'i binary'ye ya da ilk önce 10'luk sisteme, daha sonra binary'ye çevirir.

0x23 değeri atanmış bir değişkeni yazdırmak istediğinizde ise çıktı olarak 35 alırız. Hex formatında çıktı almak istiyorsanız, çıkış formatını hex yapmanız gerekir.

Mesela C'de su şekilde ekrana yazdırılı :

```c
int sayi = 35;
printf("%x", sayi);
```

Program çıktısı `23` olacaktır.
