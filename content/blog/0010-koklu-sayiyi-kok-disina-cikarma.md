---
title: "Köklü Sayıyı Kök Dışına Çıkarma"
date: "2013-07-03T21:50:16+03:00"
thumbnail: "/img/0010-koklu-sayiyi-kok-disina-cikarma.png"
categories: ["C"]
tags: ["C"]
url: "koklu-sayiyi-kok-disina-cikarma"
---

### Açıklama :
Üniversiteye hazırlanırken çok lazım olduğu için yazıp arkadaşlara dağıtmıştım. Şahsen ben de çok kullanmıştım. :) 

Not : Kök içindeyi ifade etmek için / karakteri kullanılacaktır. Ör: Kök içinde 16 = /16

### Örnek :
Kullanıcının 1800 değerini girdiğini düşünelim. /1800 demek /(30*30*2) demektir. Yani bu durumda /(30^2*2) oluyor ve bu /(30^2)*/2 şeklinde ayrılabiliyor (Matematik kuralı). Bu durumda /(30^2) kök dışına 30 diye çıkar ve geriye /2 kalır. Sonuç 30/2 olur ve bu sonucu bulduktan sonra döngüye devam etmemize gerek yoktur.

### Kaynak Kod :
{{< highlight c >}}
#include <stdlib.h>
#include <stdio.h>

int main()
{
    int n=0, s=0;

    do
    {
        printf("Koklu sayiyi girin : ");
        scanf("%d", &n);

        for(s=30; s>0; s--)
        {
            if(n%(s*s)==0)
            {
                if(s*s==n)
                {
                    printf("%d", s);
                    break;
                }
                else
                {
                    printf("%d/%d", s, (n/(s*s)));
                    break;
                }
            }
        }
        printf("\n\n\n");
    }while(n!=-1); 

    return 0;
}
{{< /highlight >}}
