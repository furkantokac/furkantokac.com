---
title: "C Simpletron - Kendi Programlama Dilinizi Oluşturun"
date: "2013-08-28T22:17:37+03:00"
thumbnail: "/img/0014-c-simpletron-kendi-programlama-dilinizi-olusturun.png"
categories: ["C"]
tags: ["C", "Simpletron"]
url: "c-simpletron-kendi-programlama-dilinizi-olusturun"
---

### Açıklama :

Kodlamayla uğraşanların çoğu, bir programlama dilinin nasıl yazıldığını kesinlikle merak etmiştir. Bu örnek ile bir programlama dilinin nasıl yazıldığını en kolay biçimde kavrayacaksınız.

`Edit V1.1 (25.11.2013)` <br>
* Yeni kod eklendi. (12 numaralı kod) <br>
* Bazı istisnalar işlendi. <br>
* Kod temizlendi. <br>


### Kaynak Kodu :

[V1.1 Program kodlarını görebilmek için tıklayın. (Kaynak Kod)</a>](https://github.com/furkantokac/Simpletron/blob/master/src/simpletron.c)


### Simpletron Dili İle Yazılmış Programlar :

[1- Kullanıcı tarafından girilen sayıların en büyüğünü bulmak. (Kaynak Kod)](http://furkantokac.com/simpletron-en-buyuk-sayi/) </br>
[2- Kullanıcı tarafından girilen 10 sayının toplamı. (Kaynak Kod)](http://furkantokac.com/simpletron-10-degerin-toplami/)


### Program Açıklaması

Örnek üzerinden gidelim. <br>
**Not** : 4 rakamlı kodların ilk 2 rakamı komutu, son 2 rakamı konumu temsil etmektir. Mesela; <br>
`4251 -- Komut 42 -- Konum 51`

![a](/img/0014-c-simpletron-kendi-programlama-dilinizi-olusturun-1.png)

Programımızı başlattığımızda karşımıza en üstteki uyarılar çıkıyor ve program bizden kod almaya hazır bir şekilde bekliyor. `0 ? (girilecekKod)` Yazdığımız her kod, integer bir dizide tutuluyor. `0 ?` ifadesindeki `0`, o an yazdığımız kodun dizinin kaçıncı elemanında tutulacağını belirtiyor. Yani `DiziElemanı ?` şeklinde. Aslında burada yazdığımız kodların dizide değil RAM’de tutulduğunu düşünmelisiniz. Dizi, bu programda RAM görevi görüyor. Dizinin elemanlarını ise RAM’in konumu olarak düşünün. Sonuçta sıfırdan bir programlama dili yazıyoruz. Ortada dizi yok RAM var :)

Şimdi bu kodlardan bahsedeyim.

* `0 ? ----` Mesela resimdeki ilk kod olan “1010” kodunu ele alalım. Buradaki yapı şu şekilde : Kod,ramKonumu. 3254 kodunda 32 rakamı komut, 54 ise bu komutun uygulanacağı RAM konumudur. Şimdi “1010” koduna geri dönelim. 10 komutu, bir değişken atamak için kullanılıyor. Yani bu kodumuzda kullanıcıdan bir değişken alıp bunu 10 konumuna atamışız.
* `1 ? ----` Daha sonra “1110” kodu geliyor. 11 komutu, yazının aşağısında da göreceğiniz gibi belirtilen konumdaki değişkeni okumaya yarıyor. Yani bu kodda 10 konumundaki değişkeni oku demişiz.
* `2 ? ----` Daha sonra ise “9999” kodu geliyor. 99 komutu programı bitirmeye yarıyor. Programı bitirmek için bir konum belirtmek gerekmiyor. Bu yüzden ister 9934 yazın ister 9900 yazın. 99 komutunu kullanacaksanız önemli olan koddaki ilk 2 rakam.
* `3 ? ----` "-9999" ile de kod girişini kesiyoruz. Şimdi diyeceksiniz “99 komutunu girdikten sonra zaten programı bitirmiş oluyoruz. Neden bir daha -9999 giriyoruz ?” Açıkçası ben de programı ilk başta 99 komutunu gördüğü an kod girişini kesecek şekilde yazmıştım fakat daha sonra fark ettim ki, -9999 ile yaparsam program daha esnek oluyor. Şuan 2’den fazla 99 komutu girebiliyorum. Bir de bilmenizde yarar var, -9999 RAM’e(diziye) kayıt edilmiyor.


### Bu Programlama Dilini Nasıl Yazdık ?

Program `kodGir` ve `calistir` adında iki ana fonksiyondan oluşuyor.

- İlk olarak `kodGir` fonksiyonu çalışıyor ve bu sayede programı çalıştırdığımızda integer dizimize kodlarımızı teker teker giriyoruz. (Burada bazı ek fonksiyonlar konsol ekranında kod girerken kolaylık sağlıyor ve girdiğimiz kodları kontrol ediyor.)
- Daha sonra `calistir` fonksiyonu çalışıyor ve bu dizimizdeki kodları okuyarak, gerekli `case` içeriğini çalıştırıyor. Mesela dizimizde “1011” kodu bulunuyor. Bu kodun ilk iki rakamı komut, son iki rakamı ise konumdur. Bunlara ayrı ayrı ihtiyacımız olduğu için ayırmamız gerekiyor. Bunu da ufak bir matematik sihri ile hallediyoruz. İlk olarak komutRegister değişkenine, dizimizdeki kodu aktarıyoruz. Daha sonra kod değişkenine komutRegister / 100 yaparak ilk iki rakamı aktarıyoruz. Düşünün. 1011/100 = 10 çıkıyor. 11 kalanı, değişkenimiz integer türünde olduğu için ihmal ediliyor. Dolayısıyla “kod” değişkenimize kodumuzu aktarabiliyoruz. “konum” değişkenine de aynı mantıkla komutRegister % 100 yaparak ilk 2 rakamı aktarıyoruz. 1011 % 100 = 11 çıkıyor. Burada birşeyi ihmal etmemiz gerekmiyor :) Her döngüde komutSayici’yı 1 arttırarak dizimizdeki bir sonraki elemana, dolayısıyla bir sonraki komuta geçiyoruz. Burada şöyle bir istisna karşımıza çıkıyor. Dallanma yapacaksak komutSayici’yi 1 arttırmamız, mantık hatasına neden oluyor çünkü “dallan” komutu ile direkt olarak gitmek istediğimiz kodu yazıyoruz. Bu durumu düzeltmezsek gitmek istediğimiz konum yerine, gitmek istediğimiz konumdan bir sonraki konuma gitmiş oluyoruz. Bu yüzden dallanma kodlarından sonra "komutSayici--" yaptım ki, 1 artırıldıktan sonra istediğim kod yerine istediğim kodun 1 sonraki koduna gitmesin.


### Akümülatör Nedir ?

Komutlara geçmeden önce kısaca değineyim ki aklınız karışmasın.
Akümülatör, RAM'de sabit bir konumda tutulan değişkendir. Mesela Simpletron(Yazdığımız dil)'da toplama işlemi yapacaksanız algoritmanız şu şekilde olacak :

* `0 ?` Kullanıcıdan alınan sayıyı belirtilen konuma yaz.
* `1 ?` Kullanıcıdan alınan sayıyı belirtilen konuma(Öncekinden farklı bir konum) yaz.
* `2 ?` Kullanıcıdan alınan sayılardan bir tanesini (istediğinizi) akümülatöre yaz.
* `3 ?` Akümülatördeki sayı ile belirtilen konumdaki sayıyı topla ve sonucu akümülatöre yaz.
* `4 ?` Akümülatördeki sayıyı belirtilen konuma yaz.
* `5 ?` Akümülatördeki sayıyı yazdığımız konumu ekrana yazdır.

Peki neden direk "1. sayı ile 2. sayıyı topla" demedik ? Çünkü yazacağımız kodlar 4 rakamlı olmak zorunda ve bu işlemi de 4 rakamla yapmamız olanaksızdır.


### Tüm Komutlar

Şimdi gerekli bilgileri öğrendiğinize göre tüm komutları inceleyebilir, istediğiniz gibi komut ekleyip, çıkartarak dilinizi daha da geliştirebilirsiniz.

10 //Belirtilen konuma kullanıcıdan alınan değeri ata </br>
1052 : 52 konumuna kullanıcıdan aldığı değeri depolar.

11 //Belirtilen konumdaki değişkeni ekrana yaz </br>
1152 : 52 konumundaki değeri ekrana yazdırır.

12 //Belirtilen konuma bir değer ata </br>
1240 : 40 konumuna bir değer ata. int konum = 20; gibi.

20 //Akümülatör = belirtilenKonumdakiDeğer </br>
2052 : Akümülatöre 52 konumundaki değeri yükler.

21 //Akümülatördeki değeri belirtilen konumda sakla </br>
2125 : Akümülatördeki değeri 25 konumuna yazar.

30 //akümülatör = akümülatördekiDeğer + belirtilenKonumdakiDeğer </br>
3015 : 15 Konumundaki değer ile yukarıdaki işlemi yapar ve sonucu akümülatöre yazar.

31 //akümülatör = akümülatördekiDeğer – belirtilenKonumdakiDeğer </br>
3115 : 15 Konumundaki değer ile yukarıdaki işlemi yapar ve sonucu akümülatöre yazar.

32 //akümülatör = akümülatördekiDeğer / belirtilenKonumdakiDeğer </br>
3215 : 15 Konumundaki değer ile yukarıdaki işlemi yapar ve sonucu akümülatöre yazar.

33 //akümülatör = akümülatördekiDeğer * belirtilenKonumdakiDeğer </br>
3315 : 15 Konumundaki değer ile yukarıdaki işlemi yapar ve sonucu akümülatöre yazar.

34 //akümülatör = akümülatördekiDeğer % belirtilenKonumdakiDeğer </br>
3422 : 22 Konumundaki değer ile yukarıdaki işlemi yapar ve sonucu akümülatöre yazar.

35 //akumulatör = akümülatör^belirtilenKonumdakiDeğer </br>
3536 : 36 konumundaki değeri üs, akümülatörü iste taban yaparak üs alır ve sonucu akümülatöre yazar.

38 //belirtilenKonumundakiDeger-- </br>
3821 : 21 konumundaki değeri 1 azaltır.

39 //belirtilenKonumundakiDeger++ </br>
3920 : 20 konumundaki değeri 1 arttırır.

40 //Belirtilen konuma git/dallan </br>
4012 : 12 konumunu gider.

41 //Akümülatör negatifse belirtilen konuma dallan </br>
4130 : Akümülatör negatifse 30 konumuna gider.

42 //Akümülatör pozitifse belirtilen konuma dallan </br>
4230 : Akümülatör pozitifse 30 konumuna gider.

43 //Akümülatör sıfır ise belirtilen konuma dallan </br>
4330 : Akümülatör sıfır ise 30 konumuna gider.

99 //Programı bitir </br>
9900
