---
title: "Çarpma Alıştırması"
date: "2013-07-03T19:41:32+03:00"
thumbnail: "/img/0007-carpma-alistirmasi.png"
categories: ["C Programlama"]
tags: ["c"]
url: "carpma-alistirmasi"
---

### Açıklama :
Çarpmayı yeni öğrenen ilk okul öğrencilerinin alıştırma yapabileceği ufak bir program. Talep olursa toplama, çıkarma, çarpma, bölme seçenekleri ekleyip, arayüz yazıp programı geliştirebilirim :)

### Kaynak Kod :
{{< highlight c >}}
#include <stdio.h>
#include <time.h>

int main()
{
    int life=10, digit=1, first=0, second=0, answer=0, score=0;

    srand(time(NULL));
    
    printf("Kac basamakli sayilarla alistirma yapmak istiyorsunuz : ");
    scanf("%d", &digit);
    
    do{
        first  = 1 + rand()%(digit*10);
        second = 1 + rand()%(digit*10);
        
        printf("%d x %d = ", first, second);
        scanf("%d", &answer);
        
        if(answer == (first*second))
        {
            score++;
            printf("\tCAN : %d\tPUAN : %d\n", life, score);
        }
        else
        {
            life--;
            printf("\tCAN : %d\tPUAN : %d\n", life, score);
        }
    }while(life>0);
    
    printf("\nGame OVER!\nPuanin : %d.\n", score);

    return 0;
}
{{< /highlight >}}
