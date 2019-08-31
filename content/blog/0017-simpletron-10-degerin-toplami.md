---
title: "Simpletron 10 Değerin Toplamı"
date: "2013-11-25T23:45:12+03:00"
thumbnail: "/img/0017-simpletron-10-degerin-toplami.png"
categories: ["C Programlama"]
tags: ["c", "simpletron"]
url: "simpletron-10-degerin-toplami"
---

<h3>Açıklama :</h3>
Programın amacı, kullanıcı tarafından ardı ardına girilen 10 sayıyı toplayıp ekrana yazdırmak.
Not : Program, yukarıdaki resimde görülen 25 konumuna aldığı sayı kadar dönecektir.
<h3>Kod İnceleme :</h3>
&gt;20 konumundaki değer : Sayaç.
<div>&gt;21 konumu : kullanıcıdan alınan değerin tutulduğu konum.</div>
&gt;25 konumundaki değer : dongünün tamamlanıp tamamlanmadığını kontrol etmek için kullanacağımız değerdir. Ör/ while( 10 &lt; a ) --- Buradaki 10 sayısı, 25 konumundaki sayıdır.
&gt;30 konumu : Sayıların toplamının tutulduğu konum.

<b>  0 ?</b> 25 konumuda 10 değerini yaz. Bunu döngümüz için kullanacağız.
<b>  1 ?</b> 20 konumuna, döngünün her turunda 1 arttırılacak olan sayacımızı giriyoruz. 0 dan başlaması için 0 değerini atıyoruz.
<b>  2 ?</b> Kullanıcıdan bir sayı al ve bunu 21 konumuna yaz.
<b>  3 ?</b> 21 konumundaki değeri akümülatöre yükle.
<b>  4 ?</b> akümülatör = akümülatördekiSayi + 30KonumundakiDeger
<b>  5 ?</b> Akümülatördeki değeri 30 konumuna yaz.
<b>  6 ?</b> 20 konumudaki değeri 1 arttır çünkü döngü 1 tur tamamladı.
<b>  7 ?</b> 20 konumundaki değeri akümülatöre yükle.
<b>  8 ?</b> akümülatör = akümülatördekiDeğer - 25KonumundakiDeger
<b>  9 ?</b> Akümülatör negatifse 2 konumuna dallan. Akümülatördeki değerin negatif olması demek, 20 konumudaki (yani sayaç) değerin, 25 konumundaki değerden (10 sayısı) küçük olması demek.
<b>10 ?</b> 30 konumudaki değeri yazdır.
<b>11 ?</b> Programı sonlandır.
<b>12 ?</b> Kod girişini kes.
<h3>Kaynak Kod :</h3>
<pre>1225
1220
1021
2021
3030
2130
3920
2020
3125
4102
1130
9999
-9999
</pre>
