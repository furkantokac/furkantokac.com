---
title: "Tavşan Kaplumbağa Yarışı"
date: "2013-07-03T14:10:29+03:00"
thumbnail: "/img/0006-tavsan-kaplumbaga-yarisi.png"
categories: ["C Programlama"]
tags: ["c"]
url: "tavsan-kaplumbaga-yarisi"
---

### Açıklama :
Konsolda basit bir görselleştirmeyle yapılmış tavşan-kaplumbağa yarışı. Tavşanın ve kaplumbağanın adım attığı if yapılarının içindeki değerleri değiştirerek oyunu daha güzel bir hale getirebilirsiniz :)

### Kaynak Kod :
{{< highlight c >}}
#include <stdlib.h>
#include <stdio.h>
#include <time.h>

void yazdir(const int, const int);
int kontrol(const int, const int);
int bs=0;

int main(int argc, char *argv[])
{
    int kap=1, tav=1, kontroli=0, ilerleme, bakiye=100, oyun, karakter, dopK, dopT, os=2;
    srand(time(0));
    while(bakiye > 0)
    {
        dopK=4;
        dopT=4;
        
        printf("Bakiyeniz : %d\n", bakiye);
        if(bakiye > 50)
        {
            while(os > 0)
            {
                printf("Doping icin 10 veya 20 yazin.\nTavsan icin 1, kaplumbaga icin 2 yazin(doping on) : ");
                /**/ scanf("%d", &karakter);
                if(karakter == 1 || karakter == 2 || karakter == 10 || karakter == 20)
                os = -1;
                else
                printf("Sadece 1, 2, 10, 20 girebilirsiniz.\n");
            }
            os=2;
            if(karakter == 20)
            {
                bakiye -=50;
                karakter = 2;
                dopK = 4;
            }
            if(karakter == 10)
            {
                bakiye -= 50;
                karakter = 1;
                dopT = 2;
            }
            
            printf("Bakiyeniz : %d\n", bakiye);
            printf("Kac akcelik oynayacaksiniz : ");
            scanf("%d", &oyun);
            
            if(oyun > bakiye)
            {
                bakiye += 50;
                printf("Bakiyenizden buyuk akce oynayamazsiniz.\n\n");
                continue;
            }
        }
        else
        {
            while(os > 0)
            {
                printf("Tavsan icin 1, kaplumbaga icin 2 yazin : ");
                /**/ scanf("%d", &karakter);
                if(karakter == 1 || karakter == 2)
                os = -1;
                else
                printf("Sadece 1, 2 girebilirsiniz.n");
            }
            os=2;
            printf("Kac akcelik oynayacaksiniz : ");
            /**/ scanf("%d", &oyun);
            if(oyun > bakiye)
            {
                printf("Bakiyenizden buyuk akce oynayamazsiniz.\n\n");
                continue;
            }
        }
        while(kontroli == 0)
        {
            printf("\n");
            yazdir(kap,tav);
            int sayici=0;
            for(sayici =0; sayici<10000000; sayici++);
            ilerleme = 1 + rand() %10;
            if(ilerleme <= 2)
            {
                kap += 3;
                tav +=2;
            }
            else if(ilerleme <= 4)
            {
                kap += 3;
                tav += 9;
            }
            else if(ilerleme <= 5)
            {
                kap += 3;
                tav -= 12;
            }
            else if(ilerleme <= 7)
            {
                kap -= 6;
                tav += 1;
            }
            else if(ilerleme <= 8)
            {
                kap += dopK;
                tav += dopT;
            }
            else
            {
                kap += 1;
                tav -= 2;
            }
            if(tav < 0)
                tav = 0;
            if(kap < 0)
                kap = 0;
            kontroli = kontrol(kap, tav);
        }
        kontroli = 0;
        tav = 1;
        kap = 1;
        
        if(bs == karakter)
        {
            bakiye += oyun;
            printf("\nKAZANDINIZ!!");
        }
        else
        {
            bakiye -= oyun;
            printf("\nKAYBETTINIZ!!");
        }
        printf("\n\n");
    }
    
    printf("Game Over..!\n\n");
    return 0;
}

void yazdir(const int k, const int t)
{
    int i=0;
    for(i=0;
    i < 70;
    i++)
    {
        if(k == i)
            printf("K");
        else if(t == i)
            printf("T");
        else
            printf(".");
    }
}

int kontrol(const int k, const int t)
{
    if(t >= 70)
    {
        printf("\n\n\tTAVSAN KAZANDI!!!\n\n");
        yazdir(k, 69);
        bs = 1;
        return 1;
    }
    else if(k >= 70)
    {
        printf("\n\n\tKAPLUMBAGA KAZANDI!!!\n\n");
        yazdir(69, t);
        bs = 2;
        return 1;
    }
    else
    return 0;
}
{{< /highlight >}}
