---
title: "Asal Sayı Bulma"
date: "2013-07-03T13:06:40+03:00"
thumbnail: "/img/0004-asal-sayi-bulma.png"
categories: ["C Programlama"]
tags: ["c"]
url: "asal-sayi-bulma"
---

### Açıklama :
Girilen sayının asal olup olmadığını kontrol eder. Asal değilse, bölündüğü en küçük asal çarpanı yazar.

### Not :
Program math.h kütüphanesi kullanmaktadır. Linux sistemlerde math.h kütüphanesi kullanan programları derlemek için -lm parametersi kullanılmalıdır.
Ör : gcc proje.c -o proje -lm

### Kaynak Kod :
{{< highlight c >}}
#include <stdio.h>
#include <math.h>

int asal_sayi(int);

int main()
{
    int sayi=0;
    
	printf("Sayinizi Girin : ");
    scanf("%d", &sayi);
    
    sayi = asal_sayi(sayi);
    if(sayi == 0)
    	printf("Sayi ASALDIR.\n\n");
    else
    	printf("Sayi ASAL DEGILDIR.\nBolundugu sayi : %d\n\n", sayi);
	
    return 0;
}

int asal_sayi(int sayi)
{
    int bolum=3;
    
	if(sayi == 2)   return 0;
    if(sayi == 1)   return 1;
    if(sayi%2 == 0) return 2;
    
    for(bolum=3; bolum < sqrt(sayi)+1; bolum+=2)
    	if((sayi % bolum)== 0)
            return bolum;
    
    return 0;
}
{{< /highlight >}}
