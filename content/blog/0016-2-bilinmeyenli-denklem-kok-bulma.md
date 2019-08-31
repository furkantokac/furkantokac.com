---
title: "2 Bilinmeyenli Denklem Kök Bulma"
date: "2013-11-14T00:05:46+03:00"
thumbnail: "/img/0016-2-bilinmeyenli-denklem-kok-bulma.png"
categories: ["C Programlama"]
tags: ["c"]
url: "2-bilinmeyenli-denklem-kok-bulma"
---

<h3>Açıklama :</h3>
İstenilen formatta denkleminizi girin ve sonucu size versin.
Kodlamada sadece matematik mantığı kullanılmıştır. Program ilk olarak Y bilinmeyenini buluyor. Daha sonra Y'yi yerine koyarak X'i buluyor ve ekrana yazdırıyor.
<h3>Kaynak Kod :</h3>
<pre>
#include <stdio.h>

int main(int argc, char *argv[])
{
    float a=0, b=0, c=0, d=0,//Islem1 katsayıları
          e=0, f=0, g=0, h=0,//Islem2 katsayıları
          x=0, y=0,          //Bilinmeyenler
          rel1=0, bil1=0;    //İşlem1'deki bilinmeyen(bil1) ve diğerki sayının(bil1)
                             //kat sayılarını tutacak değişkenler.
    
    printf("Denklem1 : aX+b = cY+d\n");
    printf("a = "); scanf("%f", &a);
    printf("b = "); scanf("%f", &b);
    printf("c = "); scanf("%f", &c);
    printf("d = "); scanf("%f", &d);
    printf("%.3fX + %.3f = %.3fY + %.3f\n", a, b, c, d);
    
    rel1 = (d-b)/a;
    bil1 = c/a;
    
    printf("\nDenklem2 : eX+f = gY+h\n");
    printf("e = "); scanf("%f", &e);
    printf("f = "); scanf("%f", &f);
    printf("g = "); scanf("%f", &g);
    printf("h = "); scanf("%f", &h);
    printf("%.3fX + %.3f = %.3fY + %.3f\n", e, f, g, h);
    
    if(((f-h) + e*rel1) != 0)
        y = ((f-h) + e*rel1) / (g - (e*bil1));
    
    if((h-f) != 0)
        x = (h-f)/e;
    
    if((g*y) != 0)
        x = x + (g*y)/e;
    
    
    printf("\nX = %.3f\nY = %.3f", x, y);
    
    printf("\n\n");
    return 0;
}

</pre>
