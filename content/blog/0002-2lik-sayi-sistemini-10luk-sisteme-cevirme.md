---
title: "2lik Sayı Sistemini 10luk Sisteme Çevirme"
date: "2013-07-03T02:17:10+03:00"
thumbnail: "/img/0002-2lik-sayi-sistemini-10luk-sisteme-cevirme.png"
categories: ["C"]
tags: ["C"]
url: "2lik-sayi-sistemini-10luk-sisteme-cevirme"
---

### Açıklama :
Program basit bir şekilde 2lik (binary) sayı sisteminde girilen değeri 10luk (decimal) sayı sistemine çeviriyor.
Bildiğiniz gibi 2lik bir sayıyı 10luk sisteme çevirirken, ikilik sayının ilk değerini 2^0 ile çarpıyoruz. Daha sonraki değerini 2^1 ile çarpıyoruz ve bu işlemi 2lik sistemdeki sayının basamakları bitinceye kadar yapıp, çıkan sonuçları topluyoruz.

### Örnek :
**2'lik sayı** : 10101
</br>
**10'luk hali** : 2^4*1 + 2^3*0 + 2^2*1 + 2^1*0 + 2^0*1 = 21

### Kaynak Kod :
{{< highlight c >}}
#include <stdio.h> 

int main()
{ 
    int n=0, tpl=0, s=1;
    
    tpl=0;
    s=1; 
    
    printf("Ikilik sistemdeki sayiyi girin : ");
    scanf("%d", &n);

    while(n > 0)
    {
        tpl += s*(n%10);
        s *= 2;
        n /= 10;
    }

    printf("%d\n",tpl);

    return 0;
}
{{< /highlight >}}
