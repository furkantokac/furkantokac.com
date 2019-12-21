---
title: "2lik Sayı Sistemini 10luk Sisteme Çevirme"
date: "2013-07-03T02:17:10+03:00"
lastmod: "2019-12-22T00:04:09+03:00"
thumbnail: "/img/0002-2lik-sayi-sistemini-10luk-sisteme-cevirme.png"
categories: ["C"]
tags: ["C"]
url: "2lik-sayi-sistemini-10luk-sisteme-cevirme"
images: ["/img/0002-2lik-sayi-sistemini-10luk-sisteme-cevirme.png"]
---

### Açıklama

Program basit bir şekilde 2lik (binary) sayı sisteminde girilen değeri 10luk (decimal) sayı sistemine çeviriyor.

**2lik sayı 10luğa nasıl dönüştürülür ?** <br>
Bildiğiniz gibi 2lik bir sayıyı 10luk sisteme çevirirken, ikilik sayının ilk değerini 2^0 ile çarpıyoruz. Daha sonraki değerini 2^1 ile çarpıyoruz ve bu işlemi 2lik sistemdeki sayının basamakları bitinceye kadar yapıp, çıkan sonuçları topluyoruz. Örnek :

```
2lik sayı   : 11001
10luk hali  : 2^4*1 + 2^3*1 + 2^2*0 + 2^1*0 + 2^0*1 = 25
```

Yukarıda gördüğümüz gibi, aslında kod yazarken bize temel olarak gerekecek şeyler; <br>
**1.** Girilen sayının tek tek basamak değerleri. Örnekte basamak değerleri soldan sırayla : 1,1,0,0,1 <br>
**2.** 2 ^ basamakSayısı - 1. Mesela 3. basamaktaysak 2 ^ (3-1) lazım ki 2^2 * basamakDegeri işlemini yapabilelim.

**Ciddi bir projede burada anlatılan yöntemi kullanmam doğru olur mu ?** <br>
Kısaca; Programlama alıştırması olarak üzerinde uğraşmak haricinde buradaki yöntem hiç bir durumda kullanılmamalı. Açıklama; Bu örnek aslında tam olarak *2lik sayı sisteminde girilen değeri int (integer, yani tam sayı) olarak alıp 10luk sisteme çevirmek* tir. Dolayısıyla girilen 2lik sayı int'in limitlerine tâbidir. Ciddi bir projede 2lik sayılar ile uğraşmanız gerekiyorsa, aşırı verimsiz ve esnek olmadığından dolayı bu yöntem asla kullanılmamalıdır.

**Bu yöntem neden verimsiz, esnek değil ?** <br>
Bir int değer (çoğu durumda) 4byte'tır, limiti -2147483648 ile +2147483647 arasındadır. Yani int olarak tutabileceğiniz en büyük 2lik sayı 10 haneli(digit) 1111111111'dir. 2lik sayının tek hanesi 1bit'tir yani 10 haneli bir 2lik sayı aslında 10bit hafızaya ihtiyaç duyar fakat biz int kullanarak 10bit'lik sayı için 4byte\*8=32bit harcıyoruz. Bu hafıza anlamındaki eksikliği, performansına hiç değinmeyelim bile. int olarak tutulan 2lik sayılar ile işlem yapmayı, bit karşılaştırması yapmayı vs. deneyin araya ne kadar gereksiz dönüştürmeler gireceğini göreceksiniz. Halbuki bilgisayar 2lik tabanda çalışıyor, bu işlemler bu kadar zor olmasa gerek :) Bu durum biraz şuna benziyor: Ali (programcı) ile Ayşe (bilgisayar) adında 2 kişi var. Ali, hem İngilizce hem Türkçe biliyor, Ayşe sadece Türkçe biliyor. Ali ile Ayşe Türkçe konuşsalar ne güzel anlaşabilirler fakat Ali İngilizce konuşuyor, (int olarak alınan girdi(input)) Ayşe Ali'nin söylediğini çeviri programına (aşağıdaki program) yazarak anlıyor, sonra Ali'ye Türkçe cevap (çıktı(output)) veriyor. Daha verimsiz olamazdı herhalde.

**"Teklifsiz tenkit tahriple sonuçlanır", peki ne yapmalı ?** <br>
Projenin yapısına, amaca göre karar verilmesi en doğrusu olur. "c bit array" anahtar kelimesi üzerinden araştırma yapabilirsiniz. Mesela C++/Qt üzerindeki bir projede QBitArray kullanıyordum ve işlemleri bitdüzeyi(bitwise) operatörleriyle yapıyordum. Python üzerindeki bir başka projede bytearray kullanıyordum, işlem gerekmiyordu fakat bit okumak, değiştirmek için bitdüzeyi(bitwise) operatörlerini kullanıyordum. Bunun yanında sayıyı direkt int olarak tutup bitdüzeyi(bitwise) operatörler ile de kullanabilirsiniz. Tavsiye için e-posta atarsanız yardımcı olmaya çalışırım.

**Peki girdi(input) alırken illa ki int kullanmak zorunda değil miyim ? 2lik girdi nasıl alınır ?** <br>
Ciddi bir proje yapıyorsanız girdi meselesini dikkate almalısınız. Direkt scanf ile girdi okumamalısınız. Bu iş projeye göre değerlendirilmeli. "c secure input" anahtar kelimesi üzerinden araştırma yapabilirsiniz. Tavsiye için e-posta atarsanız yardımcı olmaya çalışırım.


### Kaynak Kod

{{< highlight c >}}
#include <stdio.h> 

int main()
{ 
    int sayi          = 0; // Kullanicinin girecegi ikilik sayi.
    int basamakDegeri = 0; // O anki isleyecegimiz basamagin degerini burada tutacagiz.
    int ikininKati    = 1; // Bize gerekli olan 2nin kati sayiyi burada tutacagiz.
    int toplam        = 0; // Tum toplam burada duracak.
    
    // Kullanicidan 2lik sayimizi aliyoruz
    printf("2lik sayiyi girin \t : ");
    scanf("%d", &sayi);
    
    // Burada soyle bir mantik kullanalim. Bizim islemimiz su sekilde:
    // toplam = basamak1*2^(1-1) + basamak2*2^(2-1) + basamak3*2^(3-1)...
    // Yukaridan anlasilacagi uzere sag basamaktan baslayip sola dogru gidecegiz.
    // Her dongude bana sunlar gerek; basamakDegeri, 2^(basamak-1)
    while(sayi > 0)
    {
        // "sayi"nin en sag basamaginin degerini alip dongumuzde onu isleyelim
        basamakDegeri = sayi % 10;
        
        // Eger "basamakDegeri" 0 ve 1 degilse girilen sayida problem var demektir.
        if (basamakDegeri != 0 && basamakDegeri != 1)
        {
            printf("Ikilik sayi sadece 0 ve 1'den olusur. Lutfen girilen sayiyi kontrol edin.\n");
            return 1; // main()'den 1 ile donmek programin hata ile bittigi anlamina gelir.
        }
        
        // Sayinin bir sol basamagina atlamis olmak icin "sayi"nin en sag basamagini silecegiz.
        // Or: 110/10=11. (int oldugu icin virgul iptal oluyor)
        // Basamak kalmayinca da "sayi" 0 olacak ve son tur olacak.
        sayi /= 10;
        
        // "toplam"imiza yeni hesabimizi da ekliyoruz. 2^X*basamakDegeri islemi aslinda.
        toplam += ikininKati * basamakDegeri;
        
        // "ikininKati"ni 2 ile carpmaliyiz ki bir sonraki turda kullanacagiz.
        ikininKati *= 2;
    }

    printf("10luk karsiligi \t : %d\n",toplam);

    return 0;
}
{{< /highlight >}}

Kodu bütün halde görmek isterseniz az yorumlu ve basamakDegeri kontrolsüz hali aşağıdadır;

{{< highlight c >}}
#include <stdio.h> 

int main()
{ 
    int sayi = 0, basamakDegeri = 0, ikininKati = 1, toplam = 0;
    
    printf("2lik sayiyi girin \t : ");
    scanf("%d", &sayi);
    
    while(sayi > 0)
    {
        basamakDegeri = sayi % 10;
        sayi /= 10; // Sol basamaga kay
        toplam += ikininKati * basamakDegeri;
        ikininKati *= 2;
    }

    printf("10luk karsiligi \t : %d\n",toplam);
    return 0;
}
{{< /highlight >}}
