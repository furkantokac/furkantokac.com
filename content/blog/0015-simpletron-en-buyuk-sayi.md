---
title: "Simpletron En Büyük Sayı"
date: "2013-08-28T22:34:47+03:00"
thumbnail: "/img/0015-simpletron-en-buyuk-sayi.png"
categories: ["C"]
tags: ["C", "Simpletron"]
url: "simpletron-en-buyuk-sayi"
---

### Açıklama :

Aşağıdaki programı [Simpletron](http://furkantokac.com/c-simpletron-kendi-programlama-dilinizi-olusturun/) ile çalıştırmanız gerekiyor. Programın amacı, kullanıcı tarafından girilen sayıların en büyüğünü bulmak. Fakat kaç sayı gireceğini kullanıcı karar verecek. Kullanıcıdan alınacak ilk sayı, kaç sayı gireceği olacak.
Örnek/ ilk olarak 12 girildiyse, 12 tane sayı okuyarak bunların en büyüğünü bulacak ve programın sonunda bu sayıyı konsola yazdıracak.


### Kod İnceleme :

Not: 50 konumundaki sayı en büyük sayı, 49 konumundaki sayı kullanıcının her turda girdiği sayı, 48 konumudaki sayı kullanıcının kaç sayı gireceği bilgilerini tutan sayı.

{{< highlight js >}}
 0 ? Kullanıcıdan bir sayı al ve bunu 48 konumuna yaz. Bu sayı kullanıcının kaç sayı gireceğini gösteriyor.
 1 ? 48 konumundaki sayıyı bir azalt. Aslında burası döndümüzün başlangıcı. Devam edelim, anlayacaksınız.
 2 ? Kullanıcıdan bir sayı al ve bunu 50 konumuna yaz.
 3 ? Kullanıcıdan bir sayı al ve bunu 49 konumuna yaz.
 4 ? Akümülatöre 49 konumundaki sayıyı yükle.
 5 ? akümülatör = akümülatördekiSayi - 50KonumundakiSayi
 6 ? Eğer akümülatör negatif bir sayı ise 9 konumuna git. Akümülatörün negatif olması demek, 50 konumudaki sayının daha büyük olması demektir. Bu koşul sağlanırsa 7 ve 8. konumdaki kodları okumayacak. Bu aslında "if" ifadesidir. 7 ve 8. konum da bu "if" ifadesinin süslü parantezleri arasında kalan kısmıdır.
 7 ? 49 konumudaki sayıyı akümülatöre yükle. İçeri girdiğimize göre koşul sağlanmış demek oluyor. Yani 50 konumundaki sayıyı 49 konumundaki sayı ile değiştirmeliyiz çünkü 49 konumundaki daha büyük.
 8 ? Akümülatördeki sayıyı 50 konuma yaz.
 9 ? 48 konumundaki sayıyı bir azalt. Çünkü döngü, 1 turu tamamladı.
10 ? Akümülatöre 48 konumudaki değeri yükle. Bu ve bundan sonraki 2 adımı yapmamızın amacı, 48 konumuda tutulan değerin sıfır olup olmadığını öğrenebilmek içindir. Çünkü eğer sıfır olmuşsa döngüden çıkacağız.
11 ? Akümülatör 0 ise 13 konumuna dallan. Bunu yaparak döngüden çıkmış olacağız. Değilse devam.
12 ? 3 konumuna dallan. Bu sayede döngümüzün başına dönerek aynı işlemleri tekrardan yapacağız.
13 ? 50 konumundaki sayıyı yazdır. Çünkü programımız bitti ve en büyük sayımız 50 konumunda tutuluyor.
14 ? Programı sonlandır.
15 ? Kod girişini kes.
{{< /highlight >}}


### Kaynak Kod :

{{< highlight js >}}
 0 ? 1048
 1 ? 3848
 2 ? 1050
 3 ? 1049
 4 ? 2049
 5 ? 3150
 6 ? 4109
 7 ? 2049
 8 ? 2150
 9 ? 3848
10 ? 2048
11 ? 4313
12 ? 4003
13 ? 1150
14 ? 9999
15 ? -9999
{{< /highlight >}}
