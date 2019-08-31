---
title: "Project-Maryo Rehber V1.0 : Başlangıç Seviyesi Python Projesi"
date: "2018-01-26T03:48:59+03:00"
thumbnail: "/img/0034-project-maryo-rehber-v01.png"
categories: ["Python"]
tags: ["python", "kolay-kodlama"]
url: "project-maryo-rehber-v01"
---

## Project-Maryo Nedir ?
Project-Maryo, bildiğimiz Mario’nun basitleştirilmiş versiyonudur. Bu projenin amacı, giriş+ seviyesindeki hackerların severek yapabileceği, yaparken de bir çok konuda bilgi sahibi olabileceği bir proje ortaya koymaktır. Projeyi yaparken bir an önce bitirmeye odaklanılmamalıdır. Öğrenmeye, mantığı kavramaya odaklanılmalıdır. Okumakta olduğunuz rehber sadece yol göstericidir. Rehberin yanında internetten yapılacak araştırmalar ile konular hakkında daha derin bilgi edinilebilir. Rehber, Python, PyCharm ve PyGame üzerinden ilerlemektedir.

**UYARI** : Bu proje bir “kodlama ödevi” değildir. Lütfen bu projeyi gerçekleştirirken, mümkün olduğunca kendinizden bir şeyler katın. Burada söylenileni aynen yapmak zorunda değilsiniz, resim çizer gibi kod yazmaya çalışın. Yazdığınız kod, ona baktığınızda sizi mutlu etsin :) bkz. [Hackers & Painters](http://t0.gstatic.com/images?q=tbn:ANd9GcTh-_SKpdBYUul3CQrrs1iV6E3bxvnddOuO3Mfg9kgz5FTiYLI9)

![](https://raw.githubusercontent.com/furkantokac/Project-Maryo/master/extras/maryov1.0.gif)

</br>
## Python, PyCharm ve PyGame Kurulumu
**[1] Python kurulumu** </br>
Python 2 veya Python 3 kurabilirsiniz. Hangisini kuracağınız tamamen sizin isteğinize ve alışkanlığınıza bağlıdır. Kararsızsanız 3 kurabilirsiniz. Linux kullanıyorsanız Python büyük ihtimal yüklüdür. Windovz :( kullanıyorsanız, [python.org](https://www.python.org/") sitesinden indirip kurabilirsiniz.

**[2] PyCharm geliştirme ortamının kurulumu** </br>
Sitesinden indirip kurabilirsiniz: [https://www.jetbrains.com/pycharm/](https://www.jetbrains.com/pycharm/)

**[3] PyGame kurulumu** </br>
Bu aşamada PyCharm’a girip projenin oluşturmuş olunması gerek. PyCharm’dan projeyi oluşturduktan sonra projeyi açıp, [buradaki](https://www.jetbrains.com/help/pycharm/installing-uninstalling-and-upgrading-packages.html) “To install a package” kısmında yazan adımları izleyin. Paket arama kısmına “pygame” yazarsanız pygame modülünü göreceksiniz. Tek tık ile kolay bir şekilde kurulum yapabilirsiniz.

</br>
## H{ACK} TIME !
Proje aşama aşama gerçekleştirilecek ve her bir aşamada, projeye ana özelliklerinden biri eklenecek. Projenin sonunda, sağa sola gidebilen, zıplayabilen bir karakterimiz olacak. Bölüm tasarımında ise, karakterin üzerinde durabileceği zemin ve karakterin düşünce öleceği boşluk kısımlar olacak. Bu özellikler kullanılarak çok farklı bölümler, hatta oyunlar yapılabilir. Bonus olarak basit bir level editörünün nasıl yapılabileceği hakkında bilgi verilecek. Proje sonunda, geliştirilebilir yapıya sahip bir temel ve bu temeli kullanabilecek bilgiye sahip olacağız. Buradan sonra kendi oyununuzu yapmamak için hiç bir neden yok. Bir nevi projenin sonu, kendinize ait daha büyük bir projenin başlangıcı olacak :) Bonus 2 kısmında projeye ne tür geliştirmeler uygulanabileceğine hakkında fikirler olacak.

PyGame hakkında dökümantasyona [buradan](https://www.pygame.org/docs/) ulaşabilirsiniz.

[maryolib.py](https://github.com/furkantokac/Project-Maryo/blob/master/starter-pack/maryolib.py) dosyasını, sizin kendi Python dosyanız ile aynı dizine koyup projenize “import”lamanız gerekmektedir. [maryolib.py](https://github.com/furkantokac/Project-Maryo/blob/master/starter-pack/maryolib.py) dosyasında projenin karışıklığını azaltmak için yazılmış bazı kodlar mevcuttur.

[maryolib.py](https://github.com/furkantokac/Project-Maryo/blob/master/starter-pack/maryolib.py) ve başlangıç için gerekli diğer dosyalara aşağıdaki linkten ulaşabilirsiniz. Dosyayı indirin ve bir dizine çıkartın. “[starter-pack](https://github.com/furkantokac/Project-Maryo/tree/master/starter-pack)” adlı klasörün içinde gereli tüm dosyalar bulunmaktadır. [github.com/furkantokac/Project-Maryo/archive/master.zip](https://github.com/furkantokac/Project-Maryo/archive/master.zip)

##### [00] PyGame temel yapısı ve ilk temas
Aşağıdaki kodun iyi incelenerek her satırının anlamaya çalışılması gerekiyor. “`print (event)`” kısmına dikkat edin ve event’in ne olduğunu kavramaya çalışın. Farklı şeyler denemekten, kodu değiştirmekten korkmayın! Örnek kodları mümkün olduğunca basit yazmaya çalıştım. Kodu fonksiyonlara, hatta objelere bölerek çok daha güzel bir program elde edebilirsiniz.

{{< highlight python >}}
import pygame, sys
import maryolib

pygame.init()

# Ekranimizi olusturuyoruz
DISPLAY_X = 800
DISPLAY_Y = 480
main_display = pygame.display.set_mode((DISPLAY_X, DISPLAY_Y), 0, 32)
pygame.display.set_caption("Project-Maryo")

# Oyunun kac fps'de calisacagini ayarlayacagiz
clock = pygame.time.Clock()
FPS = 60

# RGB renk tanimlamalarimiz
BLACK = (0, 0, 0)
GREEN = (0, 255, 0)
WHITE = (255, 255, 255)
BLUE = (0, 0, 255)
RED = (255, 0, 0)

# Beyaz rengi arka plana uygula
main_display.fill(WHITE)

# Ekranda gorulecek grafiksel objeleri bu listede tutacagiz
graph_list = pygame.sprite.Group()

# Programin ana dongusu burasi
while True:
    # Event demek bir tusa basilmasi vs. demek. Eventler pygame.event.get()
    # fonksiyonunun dondurdugu listede birikiyor. Bunlari tek tek isliyoruz.
    for event in pygame.event.get():
        # Eger pencere kapatilmissa asagiyi calistir
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

        # Bir tusa basilirsa asagiyi caltir
        if event.type == pygame.KEYDOWN:
            # Eger basilan tus esc ise bunu yap
            if event.key == pygame.K_ESCAPE:
                pygame.quit()
                sys.exit()
            # Eger basilan tus "space" tusu ise bunu yap
            elif event.key == pygame.K_SPACE:
                print("Space tusuna basildi.")

    # Event'taki kisim, butona bastiginiz an 1 kere calisir. Asagidaki kisim
    # butona basili tuttugunuz surece calisir.
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT]:
        print("Sol ok tusuna basili tutuluyor.")
    if keys[pygame.K_RIGHT]:
        print("Sag ok tusuna basili tutuluyor.")

    # Onceki ekrani tamamen temizle
    main_display.fill(WHITE)
    # Yukaridaki degisiklikleri ekrana yansitmak icin asagidaki kodlari girmeliyiz
    graph_list.draw(main_display)
    pygame.display.update()

    # Belli bir sure bekleyecegiz
    clock.tick(FPS)
{{< /highlight >}} <!-- END CODING -->

##### [01] Grafik oluşturmak ve kullanmak
Oyunu yaparken ekrandaki her şey bu aşamada yaptığımız şekilde resimlerden oluşacak.

<!-- BEGIN CODING --> {{< highlight python >}}
import pygame, sys
import maryolib

pygame.init()

# Ekranimizi olusturuyoruz
DISPLAY_X = 800
DISPLAY_Y = 480
main_display = pygame.display.set_mode((DISPLAY_X, DISPLAY_Y), 0, 32)
pygame.display.set_caption("Project-Maryo")

# Oyunun kac fps'de calisacagini ayarlayacagiz
clock = pygame.time.Clock()
FPS = 60

# RGB renk tanimlamalarimiz
BLACK = (0, 0, 0)
GREEN = (0, 255, 0)
WHITE = (255, 255, 255)
BLUE = (0, 0, 255)
RED = (255, 0, 0)

# Beyaz rengi arka plana uygula
main_display.fill(WHITE)

# Ekranda gorulecek grafiksel objeleri bu listede tutacagiz
graph_list = pygame.sprite.Group()

# Yeni bir grafik olusturuyoruz
player_character = maryolib.MaryoImageObject(35, 50, "maryo.png")
player_character.move_to(50, 100)  # Grafigin konumunu degistir

# Grafik listemize olusturdugumuz karakteri ekleyelim
graph_list.add(player_character)

# Programin ana dongusu burasi
while True:
    # Event demek bir tusa basilmasi vs. demek. Eventler pygame.event.get()
    # fonksiyonunun dondurdugu listede birikiyor. Bunlari tek tek isliyoruz.
    for event in pygame.event.get():
        # Eger pencere kapatilmissa asagiyi calistir
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

        # Bir tusa basilirsa asagiyi caltir
        if event.type == pygame.KEYDOWN:
            # Eger basilan tus esc ise bunu yap
            if event.key == pygame.K_ESCAPE:
                pygame.quit()
                sys.exit()
            # Eger basilan tus "space" tusu ise bunu yap
            elif event.key == pygame.K_SPACE:
                print("Space tusuna basildi.")

    # Event'taki kisim, butona bastiginiz an 1 kere calisir. Asagidaki kisim
    # butona basili tuttugunuz surece calisir.
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT]:
        print("Sol ok tusuna basili tutuluyor.")
    if keys[pygame.K_RIGHT]:
        print("Sag ok tusuna basili tutuluyor.")

    # Onceki ekrani tamamen temizle
    main_display.fill(WHITE)
    # Yukaridaki degisiklikleri ekrana yansitmak icin asagidaki kodlari girmeliyiz
    graph_list.draw(main_display)
    pygame.display.update()

    # Belli bir sure bekleyecegiz
    clock.tick(FPS)
{{< /highlight >}} <!-- END CODING -->

##### [02] Resmi hareket ettirme
Aşağıdaki kodda oluşturduğumuz "maryo", sağ ok tuşu ile sağa hareket ediyor. Siz de sol tuşa basıldığında sola gidecek şekilde kodu tamamlayın.

<!-- BEGIN CODING --> {{< highlight python >}}
import pygame, sys
import maryolib

pygame.init()

# Ekranimizi olusturuyoruz
DISPLAY_X = 800
DISPLAY_Y = 480
main_display = pygame.display.set_mode((DISPLAY_X, DISPLAY_Y), 0, 32)
pygame.display.set_caption("Project-Maryo")

# Oyunun kac fps'de calisacagini ayarlayacagiz
clock = pygame.time.Clock()
FPS = 60

# RGB renk tanimlamalarimiz
BLACK = (0, 0, 0)
GREEN = (0, 255, 0)
WHITE = (255, 255, 255)
BLUE = (0, 0, 255)
RED = (255, 0, 0)

# Beyaz rengi arka plana uygula
main_display.fill(WHITE)

# Ekranda gorulecek grafiksel objeleri bu listede tutacagiz
graph_list = pygame.sprite.Group()

# Yeni bir grafik olusturuyoruz
maryo = maryolib.MaryoImageObject(35, 50, "maryo.png")
maryo.move_to(50, 100)  # Grafigin konumunu degistir
graph_list.add(maryo)  # yeni grafigimizi listeye ekliyoruz

# Programin ana dongusu burasi
while True:
    # Event demek bir tusa basilmasi vs. demek. Eventler pygame.event.get()
    # fonksiyonunun dondurdugu listede birikiyor. Bunlari tek tek isliyoruz.
    for event in pygame.event.get():
        # Eger pencere kapatilmissa asagiyi calistir
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

        # Bir tusa basilirsa asagiyi caltir
        if event.type == pygame.KEYDOWN:
            # Eger basilan tus esc ise bunu yap
            if event.key == pygame.K_ESCAPE:
                pygame.quit()
                sys.exit()
            # Eger basilan tus "space" tusu ise bunu yap
            elif event.key == pygame.K_SPACE:
                print("Space tusuna basildi.")

    # Event'taki kisim, butona bastiginiz an 1 kere calisir. Asagidaki kisim
    # butona basili tuttugunuz surece calisir.
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT]:
        print("Sol ok tusuna basili tutuluyor.")
    if keys[pygame.K_RIGHT]:
        print("Sag ok tusuna basili tutuluyor.")
        maryo.move_right(5)  # 5 pixel saga hareket et

    # Onceki ekrani tamamen temizle
    main_display.fill(WHITE)
    # Yukaridaki degisiklikleri ekrana yansitmak icin asagidaki kodlari girmeliyiz
    graph_list.draw(main_display)
    pygame.display.update()

    # Belli bir sure bekleyecegiz
    clock.tick(FPS)
{{< /highlight >}} <!-- END CODING -->

##### [03] Temas algılama (collision detection)
Mesela bir resmi sağa doğru hareket ettirirken önünüze duvar çıktı. Ne yapmanız gerek ? Durmanız gerek. Peki nasıl ? Temas algılayarak. Aşağıdaki kodda Maryo ve duvar oluşturuluyor. Maryo’muz sağa doğru hareket ediyor fakat duvara temas ettiğinde daha ilerleyemiyor.

<!-- BEGIN CODING --> {{< highlight python >}}
import pygame, sys
import maryolib

pygame.init()

# Ekranimizi olusturuyoruz
DISPLAY_X = 800
DISPLAY_Y = 480
main_display = pygame.display.set_mode((DISPLAY_X, DISPLAY_Y), 0, 32)
pygame.display.set_caption("Project-Maryo")

# Oyunun kac fps'de calisacagini ayarlayacagiz
clock = pygame.time.Clock()
FPS = 60

# RGB renk tanimlamalarimiz
BLACK = (0, 0, 0)
GREEN = (0, 255, 0)
WHITE = (255, 255, 255)
BLUE = (0, 0, 255)
RED = (255, 0, 0)

# Beyaz rengi arka plana uygula
main_display.fill(WHITE)

# Ekranda gorulecek grafiksel objeleri bu listede tutacagiz
graph_list = pygame.sprite.Group()

# Yeni bir grafik olusturuyoruz
maryo = maryolib.MaryoImageObject(35, 50, "maryo.png")
maryo.move_to(50, 100)  # Grafigin konumunu degistir
graph_list.add(maryo)  # yeni grafigimizi listeye ekliyoruz

# Duvar olusturuyoruz
wall = maryolib.MaryoImageObject(50, 50, "wall.png")
wall.move_to(200, 100)  # Grafigin konumunu degistir
graph_list.add(wall)  # yeni grafigimizi listeye ekliyoruz

# Programin ana dongusu burasi
while True:
    # Event demek bir tusa basilmasi vs. demek. Eventler pygame.event.get()
    # fonksiyonunun dondurdugu listede birikiyor. Bunlari tek tek isliyoruz.
    for event in pygame.event.get():
        # Eger pencere kapatilmissa asagiyi calistir
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

        # Bir tusa basilirsa asagiyi caltir
        if event.type == pygame.KEYDOWN:
            # Eger basilan tus esc ise bunu yap
            if event.key == pygame.K_ESCAPE:
                pygame.quit()
                sys.exit()
            # Eger basilan tus "space" tusu ise bunu yap
            elif event.key == pygame.K_SPACE:
                print("Space tusuna basildi.")

    # Event'taki kisim, butona bastiginiz an 1 kere calisir. Asagidaki kisim
    # butona basili tuttugunuz surece calisir.
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT]:
        print("Sol ok tusuna basili tutuluyor.")
    if keys[pygame.K_RIGHT]:
        print("Sag ok tusuna basili tutuluyor.")
        # Temas algilama kismini burada yapiyoruz. Eger maryo ve wall temas etmiyorsa
        # saga gitmeye izin ver. Yoksa saga gitme.
        if not pygame.sprite.collide_rect(maryo, wall):
            maryo.move_right(5)  # 5 pixel saga hareket et

    # Onceki ekrani tamamen temizle
    main_display.fill(WHITE)
    # Yukaridaki degisiklikleri ekrana yansitmak icin asagidaki kodlari girmeliyiz
    graph_list.draw(main_display)
    pygame.display.update()

    # Belli bir sure bekleyecegiz
    clock.tick(FPS)
{{< /highlight >}} <!-- END CODING -->

##### [04] Checkpoint !
00-03 arasında öğrendiklerimiz aslında projenin temel kısmını oluşturuyor. Tüm projeyi bu kısımlarda öğrendiklerimizi farklı şekillerde kullanarak oluşturacağız. Yani asıl proje şimdi başlamakta. Yeni bir python dosyası oluşturup onun üzerinden devam edelim. Bu kısımdan sonra koddan ziyade daha çok mantık verilecek çünkü 00-03 arasında gerekli kod bilgisini aldık. Bundan sonra iş onları farklı şekillerde bir araya getirmeye bakıyor. Yani hacker resim çizmeye şimdi başlıyor :)

##### [05] Zemini ve karakteri yapma
![](/img/maryo-zemin-karakter.png)

Bu kısımın sonunda elimizde sağa sola gidebilen bir karakter, bu karakterin alt tarafında olacak bir zemin olması beklenmekte. Yani 2 grafik olacak ve bu grafiklerin boylarıyla ve konumlarıyla oynayarak yukarıdaki gibi bir görüntü oluşturacağız. Zeminin tamamını kaplamak için şöyle bir yöntem kullanabilirsiniz: `DISPLAY_X` değişkeni ekranın x düzlemdeki boyutunu belirtmektedir yani genişliğini. Ekranın genişliği kadar geniş, ve belli bir yükseklikte, mesela 50 piksel yükseklikte bir zemin oluşturabiliriz. Konumlandırırken de `DISPLAY_Y` ile ekranın yüksekliğini alabiliriz ve ekranın yüksekliği sayesinde grafiği ekranın en altına yerleştirebiliriz.

**İp ucu :**
```
floor = maryolib.MaryoImageObject(DISPLAY_X, 50, “wall.png")
floor.move_to(0, DISPLAY_Y-50)
```

##### [06] Yer çekimi uygulama
Şöyle bir şey uygulayabiliriz. Karakterimiz, zemin ile temas etmediği sürece, karakterimizi aşağı doğru hareket ettirebiliriz.

**İp ucu :**
```
if not pygame.sprite.collide_rect(karakterimiz, zemin):
    karakterimiz.move_down(1)
```

##### [07] Zıplamak
Zıplamak için biraz fizik uygulamamız gerekiyor. Gerçek hayatta nasıl oluyor ? Zıpladığımızda, ilk anki hızımız en fazla ve bu hız sıfır olana kadar git gide yavaşlıyor. Sıfır olduğunda en tepe noktadayız demek ve şimdi aşağı doğru hızlanmaya başlıyoruz. Biz bunu koda nasıl uygulayabiliriz ? İlk olarak zıplamayı algılayabilmek için programa bir tuş atamalıyız. Bu “space” tuşu olabilir. “Space” tuşuna basılması demek zıplamak demek. Şimdi bir değişken oluşturabiliriz, bu değişkene “down_speed” diyelim. Zıpladığımızda, down_speed’i eksili bir değer yaparsak, aşağı gitme hızımız eksi olur ve bunun anlamı yukarı gitmek demek aslında. down_speed değişkenimizin ilk değerini -20 yapalım ve karakterimize “down_speed kadar aşağı git” diyelim yani `maryo.move_down(down_speed)`. Bu kod satırından sonra `down_speed += 5` kodunu eklersek her bir döngüde ne olacak ? down_speed’in değeri, her bir döngüde şu şekilde olacak : -20, -15, -10, -5, 0, 5, 10, …. Yani bu sayılar ne demek oluyor ? Karakterimiz ilk başta yukarı doğru 20 hızında gidecek. Daha sonra bu hız 15’e düşecek. Böyle 0’a kadar gidecek ve sonrasında aşağı doğru hızlanmaya başlayacağız. Tam da istediğimiz gibi :) Peki aşağı doğru hızlanma ne zamana kadar devam edecek ? Tabiki karakterimiz tekrardan zemine temas edene kadar.

**İp ucu :** (kodun tamamı değildir. Mantığı anlayarak siz uygulamaya çalışın. Uygularken karşılaştığınız sorunların nedenini anlayarak düzeltmeye çalışın)
```
if pressed SPACE: maryo.down_speed = -20
maryo.move_down(self.down_speed)
maryo.down_speed += 5
if pygame.sprite.collide_rect(karakterimiz, zemin):
    self.down_speed = 0
```

##### [08] Boşluktan düşünce oyunun bitmesi
![](/img/maryo-zemin-boslugu.png)

Buradaki amaç anlayabileceğiniz üzerine zıplayabilen, sağa sola gidebilen karakterimizin aşağı düştüğünde oyunu kaybetmesidir. İlk önce grafikleri 2 zemin, 1 karakter olacak şekilde oluşturup yukarıdaki resimdeki gibi yerleştirelim. Bunun yanında ekranın en altına ekstradan oyuncu tarafından çok belli olmayacak yeni bir grafik oluşturalım. Bu grafik DISPLAY_X genişliğinde ve 5 piksel yüksekliğinde olsun ve ekranın en altına koyalım. Bundan sonra oyunumuzun ana döngüsünde, karakterimiz en alta oluşturduğumuz grafik ile temas etmiş mi etmemiş mi diye kontrol edelim. Ettiyse `print(‘GAME OVER’)`

**İp ucu :**
```
if pygame.sprite.collide_rect(karakterimiz, en_alttaki_zemin)
```

##### [09] Checkpoint ! Level sistemi
Ana projenin temelini bitirmiş bulunmaktayız. Bundan sonra projeye eklenmesi gereken en önemli özellik level sistemidir. Bunun için genellikle ekranın bir kopyası 2 boyutlu dizi olarak (python’da liste) tutulur. Bu dizide level tasarımı bulunur. Daha sonra ekrana grafik çizdirilirken ilk önce dizideki bilgiler (0:1 konumunda duvar var yani level[0][1] = “duvar”, 0:2 konumunda çiçek var yani level[0][2] = “cicek” gibi bilgiler tutulacak) ekrana çizdirilir ve sonrasında karakter çizdirilir. Mesela elimizde 100x50’lük bir ekran var diyelim. Bu ekranı dizi ile temsil etmek istediğimizde oranlama yapmamız gerekiyor çünkü pikseller üzerinde çalışmak gereksiz efor sarf etmemize neden olur. Diyoruz ki, biz bu ekranı 10x5’lik bir dizi ile temsil edelim:
```
level = [
“0 0 0 0 0 0 0 0 0 0”,
“0 0 0 0 0 0 0 0 0 0”,
“0 0 0 0 0 0 0 0 0 0”,
“0 0 0 0 0 1 1 0 0 0”,
“1 1 0 0 1 1 1 1 1 1” ]
```
Burada görülen 1’ler duvar oluyor, 0’lar ise boşluk. Şimdi oranlamamızı yapalım:
```
100 / 10 -> 10
 50 / 10 -> 5
```
Burada altın sayı 10 sayısı. Şimdi ben duvarları wall.png grafiği oluşturarak yapmak istiyorum. Her oluşturduğum duvarın eni ve boyutu 10 olmalı ki tam olarak ekrana oturabilsin. Burada anlatılan mantık kısmen [maze-in-pygame](https://pythonspot.com/maze-in-pygame) projesinde geçrekleştirilmiştir. Göz atabilirsiniz.

Buraya kadar her şey normal (umarım :) ) Ama level bu kadar kısa olmuyor. Bunun için nasıl bir yöntem geliştirilebilir ?  Buradan sonrası artık size kalmış.

##### [Bonus 1] Level editörü
Furkan Ahmet Kara’nın Arduino ve Led Matrix kullanarak [Maryo projesi](https://www.instagram.com/p/BTSIk5NB5kB) için yaptığı level editörü editlenerek kullanılabilir. Kodlara [şuradan](https://github.com/furkanahmetk/maryo) göz atabilirsiniz. Ayrıntılı bilgi README kısmında mevcut.

##### [Bonus 2] Geliştirmeye devam
- Level için dönen bir sopa yapılabilir. Normal Mario oyununda ateşten dönen sopalar vardı ya onun gibi. Ona deyince Maryo’muz ölebilir.
- Bölümü zorlaştırmak için üzerinde düşünülmüş bir bölüm yapılabilir.
- Level sistemi yapılabilir. Birinci level bitirildiğinde 2. si başlar.
- Menü yapılabilir. Menüde “Oyna”, “Ayarlar”, “Çıkış” gibi 3 seçenek olabilir.
- Mario oyunundakine benzer mantar sistemi yapılabilir. Gri renkteki dikdörtgen alındığında karakterimizin boyu uzar. Eğer zaten boyu uzunsa ateş edebilir.
- Puan sistemi yapılabilir. Sarı renkli dikdörtgenler oluşturulur, bu noktalar ile temas edildiğinde puan kazanılabilir.
- Düşman eklenebilir. Düşmanın tepe kısmına temas edildiğinde düşman yok olur ve puan kazanılır, sağ, sol veya alt kısmına temas edildiğinde maryomuz yok olur.
- Prenses yapılabilir. Bölüm sonunda prensese ulaşıldığında bölüm bitmiş sayılır.
- Çok farklı şeyler yapılabilir ! Hayal gücünüze kalmış.
