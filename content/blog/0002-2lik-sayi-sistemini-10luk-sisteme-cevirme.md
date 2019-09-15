---
title: "2lik Sayı Sistemini 10luk Sisteme Çevirme"
date: "2013-07-03T02:17:10+03:00"
lastmod: "2019-09-15T15:41:09+03:00"
thumbnail: "/img/0002-2lik-sayi-sistemini-10luk-sisteme-cevirme.png"
categories: ["C"]
tags: ["C"]
url: "2lik-sayi-sistemini-10luk-sisteme-cevirme"
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
