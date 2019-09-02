---
title: "C'de Matematik Fonksiyonlari"
date: "2013-07-03T12:47:40+03:00"
thumbnail: "/img/0003-cde-matematik-fonksiyonlari.png"
categories: ["C"]
tags: ["C"]
url: "cde-matematik-fonksiyonlari"
---

### Açıklama :
Cde en çok kullanılan matematik fonksiyonlarını bir projede topladım. Gerektiğinde bu projeyi açıp gerekli fonksiyonları kullanıyorum. (Ezbere karşıyız :) )

### Kaynak Kod :
{{< highlight c >}}
#include <stdio.h>
#include <math.h>

int main()
{
    //X'IN KOKU
    printf("sqrt(%.1f) = %.1f\n", 900.0, sqrt(900.0));
    printf("sqrt(%.1f) = %.1f\n\n", 9.0, sqrt(9.0));
    
    //E USSU X'SEL FONKSYON
    printf("exp(%.1f) = %f\n", 1.0, exp(1.0));
    printf("exp(%.1f) = %f\n\n", 2.0, exp(2.0));
    
    //X'IN E TABANINA GORE LOGARITMASI
    printf("log(%f) = %.1f\n", 2.718282, log(2.718282));
    printf("log(%f) = %.1f\n\n", 7.389056, log(7.389056));
    
    //X'IN 10 TABANINA GORE LOGARITMASI
    printf("log10(%.1f) = %.1f\n", 1.0, log10(1.0));
    printf("log10(%.1f) = %.1f\n", 10.0, log10(10.0));
    printf("log10(%.1f) = %.1f\n\n", 100.0, log10(100.0));
    
    //X'IN MUTLAK DEGERI
    printf("fabs(%.1f) = %.1f\n", 13.5, fabs(13.5));
    printf("fabs(%.1f) = %.1f\n", 0.0, fabs(0.0));
    printf("fabs(%.1f) = %.1f\n\n", -13.5, fabs(-13.5));
    
    //X'IN KENDINDEN BUYUK ILK TAM SAYIYA YUVARLA
    printf("ceil(%.1f) = %.1f\n", 9.2, ceil(9.2));
    printf("ceil(%.1f) = %.1f\n\n", -9.8, ceil(-9.8));
    
    //X'I KENDINDEN KUCUK ILK TAM SAYIYA YUVARLA
    printf("floor(%.1f) = %.1f\n", 9.2, floor(9.2));
    printf("floor(%.1f) = %.1f\n\n", -9.8, floor(-9.8));
    
    // X/Y ISLEMININ KALANINI BULUR
    printf("pow(%.1f, %.1f) = %.1f\n", 2.0, 7.0, pow(2.0,7.0));
    printf("pow(%.1f, %.1f) = %.1f\n\n", 9.0, 0.5, pow(9.0,0.5));
    
    //X'IN SINUSUNU, KOSINUSUNU, TANJANTINI HESAPLAR (XRADYAN)
    printf("sin(%.1f) = %.1f\n", 0.0, sin(0.0));
    printf("cos(%.1f) = %.1f\n", 0.0, cos(0.0));
    printf("tan(%.1f) = %.1f\n", 0.0, tan(0.0));
    
    return 0;
}
{{< /highlight >}}
