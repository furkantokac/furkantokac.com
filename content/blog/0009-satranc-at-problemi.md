---
title: "Satranç At Problemi"
date: "2013-07-03T21:10:25+03:00"
thumbnail: "/img/0009-satranc-at-problemi.png"
categories: ["C"]
tags: ["C"]
url: "satranc-at-problemi"
---

### Açıklama :
At, satranç tahtasındaki her kareye bir kere gelecek ve aynı kareye ikinci kez gelemeyecek. Bu problemi hamleler halinde çözen C programı. Daha hızlı olabilmesi için kodları temizlemekteyim. Bitince güncelleyeceğim.

eYaz() fonksiyonu, bulunan karelere kaç şekilde erişebileceğimizi tutan diziyi yazdıran fonksiyondur. 

Ekran görüntüsünde gördüğünüz dizilerden üstteki atın hamlelerinin yazıldığı dizi. Mesela oradaki 1 sayısı atın ilk başta bulunduğu konum(random bir şekilde ayarlanıyor o konum). Altındaki dizi ise o anda tahtadaki gidilmemiş yerlere kaç farklı yerden ulaşılabilir bilgisini tutuyor. Mesela oradaki bir konumda 2 sayısı varsa o konuma 2 farklı yerden erişilebiliyor demektir.

**Edit 04.07.2013** : Kodları temizledim. Şu an sayfada bulunan kodlar temiz kodlardır. 

### Kaynak Kod :

{{< highlight c >}}
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define BOYUT 8

void diziYaz(void);
void eKon(void);
void eYaz(void);
void hYap();

int tahta[BOYUT][BOYUT]={{0},{0}},
    eris[BOYUT][BOYUT] ={{0},{0}}, x=4, y=3;

int const   hx[]={2,1,-1,-2,-2,-1,1,2}, 
            hy[]={-1,-2,-2,-1,1,2,2,1};

int main(int argc, char *argv[])
{
    int s=1, kareSayi=0;
    
    //Her seferinde farkli yerde basla
    srand(time(NULL));
    x=rand()%BOYUT;
    y=rand()%BOYUT;
    
    tahta[y][x]=1;
    kareSayi = (BOYUT*BOYUT+1);

    while(s < kareSayi)
    {
        diziYaz();
        eKon();
        eYaz();
        hYap();
        ++s;
        tahta[y][x] = s;
    }

    printf("\nHamle Kalmadi.\nOyun Bitti. Iyi gunler.\n\n\n");
    return 0;
}

void diziYaz(void)
{
    int i=0, j=0;

    printf("\n");
    for(i=0; i <= BOYUT-1; i++)
    {
        for(j=0; j <= BOYUT-1; j++)
        {
            if(tahta[i][j] == 0)
                printf("*  ");
            else if(tahta[i][j] > 9)
                printf("%d ", tahta[i][j]);
            else    
                printf("%d  ", tahta[i][j]);
        }
        printf("\n\n");
    }
}
    
void eKon(void)
{
    int i=0, j=0, u=0;
    
    for(i=0; i <= BOYUT-1; i++)
    {              
        for(j=0; j <= BOYUT-1; j++)
        {
            eris[i][j]=0;
            for(u=0; u <=7; u++)
            {
                if(j+hx[u] >= 0 && j+hx[u] <= 7 && i+hy[u] >= 0 && i+hy[u] <= 7 && tahta[i+hy[u]][j+hx[u]] == 0 && tahta[i][j] == 0)
                    eris[i][j]++;
            }
        }
    }
}

void eYaz(void)
{
    int i=0, j=0;
    
    printf("\n");
    for(i=0; i <= BOYUT-1; i++)
    {
        for(j=0; j <= BOYUT-1; j++)
        {
            printf("%d ", eris[i][j]);
        }
        printf("\n");
    }
}

void hYap()
{
    int u=0, hamle=0;
    
    for(u=0; u <=7; u++)
    {
        if(y+hy[u] <= 7 && y+hy[u] >= 0 &&  x+hx[u] >= 0 &&  x+hx[u] <= 7 && tahta[y+hy[u]][x+hx[u]] == 0)
        {
            hamle = u;
            break;
        }
    }
    
    for(u=0; u <=7; u++)
    {
        if(y+hy[u] <= 7 && y+hy[u] >= 0 &&  x+hx[u] >= 0 &&  x+hx[u] <= 7 && tahta[y+hy[u]][x+hx[u]] == 0 && eris[y+hy[u]][x+hx[u]] <= eris[y+hy[hamle]][x+hx[hamle]])
        {
            hamle = u;
        }
    }
    
    x += hx[hamle];
    y += hy[hamle];
}
{{< /highlight >}}
