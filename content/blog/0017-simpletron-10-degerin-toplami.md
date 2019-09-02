---
title: "Simpletron 10 Değerin Toplamı"
date: "2013-11-25T23:45:12+03:00"
thumbnail: "/img/0017-simpletron-10-degerin-toplami.png"
categories: ["C"]
tags: ["C", "Simpletron"]
url: "simpletron-10-degerin-toplami"
---

### Açıklama :

Programın amacı, kullanıcı tarafından ardı ardına girilen 10 sayıyı toplayıp ekrana yazdırmak.
Not : Program, yukarıdaki resimde görülen 25 konumuna aldığı sayı kadar dönecektir.


### Kod İnceleme :

* 20 konumundaki değer, sayaç.
* 21 konumu, kullanıcıdan alınan değerin tutulduğu konum.
* 25 konumundaki değer, dongünün tamamlanıp tamamlanmadığını kontrol etmek için kullanacağımız değerdir. `Ör/ while( 10 < a )` kodundaki 10 sayısı, 25 konumundaki sayıdır.
* 30 konumu, sayıların toplamının tutulduğu konum.

{{< highlight js >}}
 0 ? 25 konumuda 10 değerini yaz. Bunu döngümüz için kullanacağız.
 1 ? 20 konumuna, döngünün her turunda 1 arttırılacak olan sayacımızı giriyoruz. 0 dan başlaması için 0 değerini atıyoruz.
 2 ? Kullanıcıdan bir sayı al ve bunu 21 konumuna yaz.
 3 ? 21 konumundaki değeri akümülatöre yükle.
 4 ? akümülatör = akümülatördekiSayi + 30KonumundakiDeger
 5 ? Akümülatördeki değeri 30 konumuna yaz.
 6 ? 20 konumudaki değeri 1 arttır çünkü döngü 1 tur tamamladı.
 7 ? 20 konumundaki değeri akümülatöre yükle.
 8 ? akümülatör = akümülatördekiDeğer - 25KonumundakiDeger
 9 ? Akümülatör negatifse 2 konumuna dallan. Akümülatördeki değerin negatif olması demek, 20 konumudaki (yani sayaç) değerin, 25 konumundaki değerden (10 sayısı) küçük olması demek.
10 ? 30 konumudaki değeri yazdır.
11 ? Programı sonlandır.
12 ? Kod girişini kes.
{{< /highlight >}}


### Kaynak Kod :

{{< highlight js >}}
 0 ? 1225
 1 ? 1220
 2 ? 1021
 3 ? 2021
 4 ? 3030
 5 ? 2130
 6 ? 3920
 7 ? 2020
 8 ? 3125
 9 ? 4102
10 ? 1130
11 ? 9999
12 ? -9999
{{< /highlight >}}
