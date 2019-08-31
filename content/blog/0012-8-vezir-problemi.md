---
title: "8 Vezir Problemi"
date: "2013-07-20T22:49:01+03:00"
thumbnail: "/img/0012-8-vezir-problemi.png"
categories: ["C Programlama"]
tags: ["c"]
url: "8-vezir-problemi"
---

### Açıklama :
Amacımız, sekiz tane veziri birbirini hiç bir türlü tehdit etmeyecek şekilde yerleştirmek.
Az önce oyun şeklinde yaptım. Otomatik çözme algoritmasını daha yapmadım. En kısa zamanda algoritmalı halini yapıp koyacağım.

Aklımdaki algoritmadan bahsedeyim. Kısaca şöyle : Bir fonksiyon, her yerin, eğer oraya vezir konulursa toplam kaç kareyi işgal edeceğini hesaplayıp, bunları "erisilebilirlik" adındaki iki boyutlu bir diziye kaydedecek. (Diziye kaydetmeden de yapılabilir fakat öyle olunca daha esnek oluyor program. Hem de olayı görselleştirmiş oluyorsunuz)
 Daha sonra "erisilebilirlik" dizisindeki en küçük sayı bulunup, kordinatları (dizi[y][x]) alınacak ve oraya hamle yapılacak. (En küçük sayıyı "erisilebilirlik" dizisinin içini doldururken de bulabilirsiniz fakat ayrı bir fonksiyon ile bulmak, programda esneklik sağlayacaktır)

### Kaynak Kod :
{{< highlight c >}}
#include <stdio.h>
#include <stdlib.h>

#define TAHTA 8

void iDiziYaz(int b[][TAHTA], const int boyuta, const int boyutb);
int hamle(int[][TAHTA], const int);
int kontrol(int b[][TAHTA], const int x, const int y);

int main(int argc, char *argv[])
{
    int d[TAHTA][TAHTA]={{0},{0}}, h=1;

    iDiziYaz(d, TAHTA, TAHTA);

    while(h < 9)
    {
        if(hamle(d, h))
            h++;
        iDiziYaz(d, TAHTA, TAHTA);
    }

    printf("Kazandiniz..!\n\n\n");
    return 0;
}

int hamle(int b[][TAHTA], const int s)
{
    int i=0, j=0, x=0, y=0, k=1;
    
    printf("Hamle X : ");
    scanf("%d", &x);
    printf("Hamle Y : ");
    scanf("%d", &y);
    
    if(kontrol(b, x, y))
    {
        b[y][x] = s;
        i=y;
        j=x;
        
        while(i < TAHTA-1)
            b[++i][x] = -1;
        i=y;
        while(i > 0)
            b[--i][x] = -1;
        while(j > 0)
            b[y][--j] = -1;
        j=x;
        while(j < TAHTA-1)
            b[y][++j] = -1;
        i=y;
        j=x;
        while(k)
        {
            i--;
            j--;
             if(j < 8 && i < 8 && j > -1 && i > -1)
             {
                 b[i][j]=-1;
             }else
             {
                 k=0;
             }
        }
        k=1;
        i=y;
        j=x;
        while(k)
        {
            i++;
            j++;
             if(j < 8 && i < 8 && j > -1 && i > -1)
             {
                 b[i][j]=-1;
             }else
             {
                 k=0;
             }
        }
        k=1;
        i=y;
        j=x;
        while(k)
        {
            i++;
            j--;
             if(j < 8 && i < 8 && j > -1 && i > -1)
             {
                 b[i][j]=-1;
             }else
             {
                 k=0;
             }
        }
        k=1;
        i=y;
        j=x;
        while(k)
        {
            i--;
            j++;
             if(j < 8 && i < 8 && j > -1 && i > -1)
             {
                 b[i][j]=-1;
             }else
             {
                 k=0;
             }
        }
        return 1;
    }
    else
    {
        printf("Gecersiz bir hamle girdiniz.");
        return 0;
    }
}

int kontrol(int b[][TAHTA], const int x, const int y)
{
    if(b[y][x] == 0 && x < 8 && y < 8 && x > -1 && y > -1)
        return 1;
    else
        return 0;
}

void iDiziYaz(int b[][TAHTA], const int boyuta, const int boyutb)
{
    int i=0, j=0;
    
    printf("\n\n%s %s %s %s %s %s %s %s %s\n", " ", "0", "1", "2", "3", "4", "5", "6", "7");
    for(i=0; i <= boyuta-1; i++)
    {
        printf("%d ", i);
        for(j=0; j <= boyutb-1; j++)
            if(b[i][j] >= 0)
                printf("%d ", b[i][j]);
         else
                printf("* ");
                
        printf("%d\n", i);
    }
    printf("%s %s %s %s %s %s %s %s %s\n", " ", "0", "1", "2", "3", "4", "5", "6", "7");
}
{{< /highlight >}}
