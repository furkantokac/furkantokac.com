---
title: "Raspberry Pi 3 Fastboot - 2 Saniyede Açılan Sistem"
date: "2050-09-15T20:38:09+03:00"
thumbnail: "/img/0042-rpi3-fast-boot-2-saniyede-acilan-sistem.jpg"
categories: ["Gömülü|Embedded"]
tags: ["Gömülü Linux", "Raspberry Pi", "Gömülü", "Buildroot"]
url: "rpi3-fast-boot-2-saniyede-acilan-sistem"
---

{{< goEnPost url="/rpi3-fast-boot-less-than-2-seconds" >}} <br>

Bu yazı sonunda, Raspberry Pi 3'ün 1.75 saniyede açılabilmesi için yapılması gereken ayarlamalarını öğrenmiş olacaksınız. Buna ek olarak Raspberry Pi 3 üzerinde Qt uygulamasını en hızlı şekilde çalıştırabilmek için yapılması gereken ayarlamalara da değineceğiz. Sonuç olarak, sisteme güç verildiği andan itibaren 2.83 saniyede açılan, Raspberry Pi 3 üzerinde çalışan bir Qt uygulamasına sahip olacağız.

Her şeyden önce boot optimizasyonunun kritik bazı aşamaları düşük seviyeli (donanıma bağımlı) işler oldukları için kullanacağımız kartı çok iyi tanımamız gerekiyor. Kartımız boot işlemini nasıl yapıyor, hangi dosyaları hangi sıra ile çalıştırıyor, çalıştırdığı dosyaların hangileri %100 gerekli gibi sorulara net cevaplar verebiliyor olmak gerekiyor. Bunun yanında yapılan optimizasyonlar kesinlikle teker teker yapılıp test edilmesi gerekiyor ki o işlemin kart üzerinde nasıl bir etki yaptığı açıkça görülebilsin.

Bu çalışma için kullanacağımız kart olan Raspberry Pi 3’ün boot işlemleri, diğer kartlardan biraz farklıdır. Boot işleminde, geleneksel yapıların aksine, CPU’dan ziyade GPU görev alıyor. Bu dökümantasyonu okumadan önce internet üzerinden konu hakkında temel bilgi edinmenizi tavsiye ederim. (bkz. [1][1], [9][9])

Raspberry, System on Chip (SoC) olarak Qualcomm'un kapalı kaynak bir çipini kullanmaktadır. Dolayısıyla SoC ile alakalı yazılımlar, Raspberry tarafından bize sağlanmakta. (bkz. [2][2]) Kapalı kaynak olduğu için bu yazılımlara normal yollarla etki edilememektedir. Raspberry ile boot optimizasyonu aşamalarında en fazla sıkıntı yaşanılan kısımlar bu nedenle bize sağlanan SoC dosyalarının üstlendiği kısımlarıdır. 


#### Raspberry Boot Dosyaları

Raspberry’nin boot süreci ile alakalı dosyalar ve amaçları kısaca  şöyledir;

* **bootcode.bin** : Bu dosya, Raspberry’nin üzerine üretici tarafından gömülmüş olan 1. Stage Bootloader yazılımının çalıştırdığı 2. Stage Bootloder dosyasıdır. GPU üzerinde çalışır. RAM’i aktif etmek için kullanılır. Amacı, 3. Stage Bootloader olan start.elf’i doğru bir şekilde çalıştırmaktır. 
* **config.txt** : GPU ayarlarını içinde barındıran dosyadır. start.elf tarafından kullanılır. 
* **cmdline.txt** : Kernel çalıştırılırken Kernel’e geçirilecek olan parametreleri içinde barındırır. start.elf tarafından kullanılır. 
* **.dtb** : Derlenmiş Device Tree dosyasıdır. Kart üzerindeki tüm donanımların tanımlamalarını içinde barındırır. start.elf tarafından okunarak Kernel.img dosyasını çalıştırırken kullanılır. 
* **start.elf** : bootcode.bin tarafından çalıştırılan 3. Stage Bootloader dosyasıdır. İçinde GPU sürücüsünü barındırır. Amacı, RAM’i GPU ile CPU arasında bölüştürmek, config.txt dosyasının içindeki ayarları GPU’ya uygulamak, ilgili .dtb dosyasını okuyarak gerekli ayarlamaları yapmak, cmdline.txt dosyasındaki parametreler ile birlikte Kernel.img’yi çalıştırmaktır. Bu işlemleri yaptıktan sonra kart açık kaldığı sürece GPU sürücüsü olarak çalışmaya devam eder. 
* **kernel.img** : Kernel dosyamızdır. start.elf tarafından çalıştırılır. Buradan sonra kontrol tamamen bizim elimizdedir. 
* **Temel mantık** : Raspberry’e güç verildi -> Raspberry’nin içindeki gömülü yazılım çalıştı -> bootcode.bin çalıştı -> start.elf çalıştı -> config.txt okundu -> .dtb okundu -> cmdline.txt okundu -> kernel.img çalıştı


#### Proje Gereksinimleri

Bu çalışma ile, Raspberry'e güç gitmesinden aşağıdaki gereklilikleri sağlayan uygulamanın başlamasına kadar geçen sürenin en aza indirilmesi amaçlanmıştır.

- Kart olarak Raspberry Pi 3 kullanılacak.
- Linux özelleştirmesi için Buildroot kullanılacak.
- Raspberry Pi 3'ün GPIO, UART özellikleri aktif ve Qt üzerinden kullanılabilir olacak.
- Buton ile GPIO tetikleyip LED'in yanıp sönme periyodu değiştirilebilecek.
- UART üzerinden veri alınacak.
- Alınan UART verileri Qt'de yazılmış olan arayüzde gösterilecek.


### Raspberry Boot Süreci

Raspberry'de bir Qt uygulamasının çalışması için gerekli olan aşamalar sırasıyla şu şekildedir;  <br>
**K1** - Raspberry'nin hazırlık süreci (1. & 2. Stage Bootloader) (bootcode.bin) <br>
**K2** - Linux'un hazırlık süreci (3. Stage Bootloader) (start.elf, bcm2710-rpi-3-b.dtb) <br>
**K3** - Linux'un çalışması (kernel.img) <br>
**K4** - InitSystem'in çalışması (BusyBox) <br>
**K5** - Uygulamamızın çalışması (Qt)


### <u>K1 - Raspberry'nin Hazırlık Süreci</u>

Bu kısımda kartın üzerine üretici tarafından gömülmüş bir yazılım, bootcode.bin’i çalıştırıyor. Kartın üzerine gömülü olan kısıma legal yollarla etki etmemiz mümkün olmadığı için o kısım ile ilgili işlem yapamadan bootcode.bin üzerinde çalışmaya başlıyoruz. bootcode.bin kapalı kaynak olduğu için direkt etki edemiyoruz. Geriye 2 yol kalıyor. Ya kapalı kaynak olarak bize sağlanan bootcode.bin’leri deneyerek ilerleme sağlamayı deneyeceğiz, ya da bootcode.bin bağlantılı olduğu dosyalara etki ederek ilerleme sağlamayı deneyeceğiz.(tersine mühendislik yolunu hesaba katmıyoruz.)

Raspberry’nin GIT sayfasına (bkz. [2][2]) giderek bootcode.bin’in farklı versiyonları var mı diye kontrol edince olmadığını görebiliriz. Eski versiyonları da denediğimizde hızda değişim olmadığını görüyoruz. Diğer seçeneğe geçebiliriz.

Start.elf kapalı kaynak olduğu için ona da direkt etki edemiyoruz fakat Raspberry’nin sayfasında farklı start.elf dosyaları olduğu gözüküyor: start_cd.elf, start_db.elf, start_x.elf. Bu dosyaların farkını araştırınca start_cd.elf’in start.elf’in basitleştirilmiş versiyonu olduğunu görüyoruz. Bu sürümde GPU özellikleri kırpılmış. Bizim uygulamamız grafiksel arayüz kullandığı için çalışmasına engel olabilir fakat tabi denemeden bilemeyiz. Kendi start.elf’imizi start_cd.elf ile değiştirip farkı gözlemlemek istediğimizde, bootcode.bin’in 0.5sn hızlandığını görebiliyoruz. Lakin uygulamamızı çalıştırmak istediğimizde hata ile karşılaşıyoruz. Peki hata neyden kaynaklanıyor, çözülebilir mi ?
Arayüz uygulamamız, OpenGL ES üzerinde çalışıyor ve start_cd.elf, OpenGL ES’in ihtiyaç duyduğu kadar GPU hafızası ayırmıyor. Bu sıkıntıyı aşmayı denediysek de başarılı olamadık ama zaman ayırılıp üzerine gidilirse çözülebileceğine inanıyorum.


### <u>K2 - Linux'un Hazırlık Süreci</u>

Bu kısımdaki işleri start.elf dosyası üstleniyor. Device Tree dosyasından donanım özelliklerine göre karttaki gerekli ayarlamaları yaparak, kendi ürettiği ve varsa cmdline.txt dosyasındaki parametreler ile birlikte Kernel imajını boot ediyor. Kernel boot olabilmesi için illa ki Device Tree dosyasının içindeki bazı kısımlara ihtiyaç duyar. start.elf de kapalı kaynak olduğu için direkt etki edemiyoruz fakat bu dosya ile bağlantılı olan, açık kaynak 2 dosya daha var: bcm2710-rpi-3-b.dtb, kernel.img

İlk olarak yapabileceğimiz iş, bu dosyalardan herhangi biri start.elf’yi yavaşlatıyor mu diye bakmak olabilir. Device Tree dosyasını sildiğimizde Kernel boot olmuyor. Burada önemli bir soru sorabiliriz, Kernel’in boot olmamasının sebebi start.elf’nin Device Tree dosyası olmadığı için çalışmaması mı yoksa Kernel için gerekli ayarların yapılamaması mı ? Bunu test edebilmek için bir yol düşünmeliyiz. Device Tree’ye ihtiyaç duymadan çalışabilen bir uygulama işimizi görecektir. Peki bu ne olabilir ? Raspberry Pi, genel amaçlı bir karttır ve sadece Kernel çalıştırmak için tasarlanmamıştır. (lakin öyle olsa bile sıkıntı olmazdı) Bu nedenle direkt olarak SoC’u programlayabilmemiz mümkündür. Ufak bir led yakma uygulaması yapıp, start.elf’in Kernel yerine bu uygulamayı çalıştırmasını sağlarsak Device Tree’yi silmemizin herhangi bir hız değişimi oluşturup oluşturmadığını gözlemleyebiliriz. İkinci bir seçenek olarak U-Boot derleyip Kernel yerine U-Boot’u çalıştırarak da deneyebiliriz fakat biz ilk seçeneği uygulayacağız. LED yakma uygulamasını yazıp çalıştırdığımızda görüyoruz ki, Device Tree’yi silmemiz bize 1.0sn kazanç sağlamış. Bir de Device Tree’nin varsayılan ismini (bcm2710-rpi-3-b.dtb) değiştirip deniyoruz. Hızlanma yine işe yarıyor. Anlıyoruz ki start.elf, öntanımlı olarak direkt “bcm2710-rpi-3-b.dtb” ismi ile dosyayı aramakta. Sonuç olarak gelişme gösterebilmek için Device Tree dosyasını bir şekilde yok etmemiz veya ismi değişmiş bir şekilde kullanmamız gerekiyor.

Device Tree dosyasının ismini değiştireceğimiz ilk yöntem için şöyle bir yol uygulayabiliriz; start.elf tarafından çalıştırılacak bir yazılım yazabiliriz. Bu yazılım, bir nevi LED yakmak yerine Device Tree ile beraber Kernel’i boot edebilecek bir yazılım olur. start.elf, bizim yazdığımız yazılımı çalıştırır, bizim yazdığımız yazılım da ismi değiştirilmiş Device Tree dosyası ile birlikte Kernel’i boot eder. Bunun için raspberry/linux kaynak kodundan yardım alabiliriz. (bkz. [10][10]) Tabi bu yönteme başlamadan önce diğer yöntemi de gözden geçirip arasında seçim yapmak en doğrusu olacaktır.

Diğer yöntemde Device Tree’yi bir şekilde iptal etmemiz lazım. Device Tree’nin, Kernel’i boot ederken zaruri olarak gerektiğini, start.elf için ise kesin olarak gerekmediğini testimizde gördük. Device Tree Kernel ile alakalıysa, bir şekilde Device Tree konfigürasyonlarını Kernel’e hardcoded şekilde belirtmeyi deneyebiliriz. Yani Device Tree’yi Kernel’e gömebiliriz. Device Tree hakkında detaylı bilgi edinirken Kernel için bu tarz bir seçeneğin halihazırda olduğunu görüyoruz. (bkz. [3][3] sayfa 11) Gerekli ayarları yapıp (bir sonraki kısımda bu ayarlamalar hakkındaki bilgi bulunuyor) test ettiğimizde görüyoruz ki Kernel boot olabiliyor. Şimdi durum analizi yapıp her şey yolunda mı diye bakmamız lazım.

Testlerden sonraki durum aşağıdaki gibi oluyor; <br>
- Qt uygulamamız sıkıntısız çalışıyor. <br>
- UART’ı test ettiğimizde çalışmadığını görüyoruz. <br>
- Kernel’in boot olma süresinin 0.8sn kadar uzadığını görüyoruz.

İlk olarak UART ile ilgili sıkıntının ne olduğunu bulmamız gerekiyor. UART’ın sıkıntısız çalıştığı bir Kernel’i boot edip, boot logları kaydediyoruz. Sonra UART’ın sıkıntılı olduğu, Device Tree gömülmüş olan Kernel’imizi de boot edip loglarını kaydediyoruz. (Bu loglara “dmesg” komutu ile ulaşılabiliyor.) Farklarını inceliyoruz. (bkz. [4][4]) Özellikle “Kernel command line:” ile başlayan satırda, UART’ın çalıştığı sistemde olup da çalışmadığı sistemde olmayan bir farkı görüyoruz. UART’ın çalıştığı sistemde “8250.nr_uarts=1” şeklinde bir parametre Kernel’e geçiriliyor. Biz de cmdline.txt dosyasına bu parametreyi koyarak UART’ın çalışmadığı sistemi boot ettiğimizde UART’ın artık çalıştığını görüyoruz. Bu problemi çözdük.

Diğer işimiz, Kernel’in boot süresinin yaklaşık 1.0sn uzamasının nedenini araştırmak. Yine loglardan faydalanacağız. Logları incelediğimizde görüyoruz ki normal çalışan sistemde olmayıp da geliştirdiğimiz sistemde olan, “random” kelimesini içeren bir problem var ve gecikme orada oluyor. Kernel’den random ile ilgili ayarları tek tek kapayıp deneyince sıkıntılı ayarı buluyoruz. Ayarı kapattığımızda her şeyin normale döndüğünü görüyoruz. Bu problemi de çözdük.

Sonuç olarak geliştirdiğimiz strateji sayesinde yaklaşık 2.0sn’lik bir kazanç sağladık. K2 için toplam harcanan zaman 0.25sn oldu. Burada geliştirmelerimiz devam edebilir (en basitinden Device Tree optimizasyonu yapılabilir) fakat daha büyük sıkıntılarımızın olduğu yerlere zaman harcamamız, az zamanda daha fazla yol kat etmemizi sağlayacaktır. Bu nedenle bir sonraki adıma geçiyoruz.


### <u>K3 - Linux'un Çalışması</u>

Aslında K2’de Kernel optimizasyonumuzun bir kısmını anlattık. Bu kısımda, değiştirdiğimiz ayarları ekleyeceğiz. Bu ayarlar hakkında bilgi almak için lütfen projenin GIT sayfasını ziyaret ediniz (bkz. [5][5]) ve gerektiğinde internet üzerinden detaylı araştırma yapınız.

**Aktifleştirdiğimiz Özellikler**
```
ARM_APPENDED_DTB
ARM_ATAG_DTB_COMPAT
```

**Pasifleştirdiğimiz Özellikler**
```
NET
SOUND
HW_RANDOM
ALLOW_DEV_COREDUMP
STRICT_KERNEL_RWX
STRICT_MODULE_RWX
NAMESPACES
FTRACE
USB_SUPPORT
BLK_DEBUG_FS
DEBUG_BUGVERBOSE
DEBUG_FS
DEBUG_MEMORY_INIT
SLUB_DEBUG
PRINTK
BUG
DM_DELAY
ELF_CORE
KGDB
PRINT_QUOTA_WARNING
AUTOFS4_FS
MEDIA_DIGITAL_TV_SUPPORT
MEDIA_ANALOG_TV_SUPPORT
MEDIA_CAMERA_SUPPORT
MEDIA_RADIO_SUPPORT
INPUT_MOUSEDEV
INPUT_JOYDEV
INPUT_JOYSTICK
INPUT_TABLET
INPUT_TOUCHSCREEN
IIO
RC_CORE
HID_LOGITECH
HID_MAGICMOUSE
HID_A4TECH
HID_ACRUX
HID_APPLE
HID_ASUS
```


### <u>K4 - InitSystem'in çalışması (BusyBox)</u>

Init system aslında ciddi bir zaman harcamıyor fakat en hızlı çalışan kod, çalışmayan koddur. :) Bu nedenle BusyBox’ı aradan çıkardık. Burada yapılan ve bizim için gerekli olan tek işlemi, GPIO ve UART işlemlerinden dolayı “File System Mounting” işlemidir. Bu işlemi basit bir şekilde kendi uygulamamıza gömdük.

Tek yapmamız gereken şu kod parçasını Qt programımızın herhangi bir yerinde çalıştırmak: <br>
`QProcess::execute("/bin/mount -a");`

Tabiki kullanıcıya arayüzün yansıtılmasını yavaşlatmayacak şekilde doğru bir yere gömmek bizim için en verimlisi olacaktır zira Mounting işlemi zaman alan bir işlemdir. Bu işlemi yaptıktan sonra uygulamamızı “init” ismi ile birlikte “/sbin/” klasörü içine koyduğumuzda işlem tamamdır.

Kernel, Userspace’i yüklediğinde her şeyden önce /sbin/init dosyasını otomatik olarak çalıştırır. Dolayısıyla uygulamamız her şeyden önce çalışmış olacak.


### <u>K5 - Uygulamamızın çalışması (Qt)</u>

Qt Creator bize detaylı hata ayıklama araçları sunuyor. Bu araçları kullanarak Qt uygulamamızın açılmasını en çok yavaşlatan unsurun ne olduğunu tespit edebiliyoruz.

**Statik Derleme** <br>
K5’te yaptığımız en önemli geliştirmelerin başında statik derleme gelmektedir. Statik derleme, uygulamanın çalışabilmesi için gerekli olan tüm kütüphanelerin kendi içinde bulunması demektir. Statik derleme işlemi için özel bazı adımlar uygulamak gerekiyor. (bkz. [6][6], [7][7]) Normal derleme işlemi yaptığımızda uygulama dinamik derlenir. Dinamik derlemede ise, uygulama ihtiyaç duyduğu kütüphaneleri sistem dosyalarından tek tek çağırır, dolayısıyla zaman kaybı meydana gelir. Statik derlemenin bizim kullanım senaryomuzda bir dezavantajı yoktur. Aksine boyut, hız gibi avantajları vardır. Yani işlerimizi kısıtlayacak bir durum yok. Bu geliştirme bize yaklaşık olarak 0.33sn kazanç sağladı.

**Budama (Stripping)** <br>
Budama işlemi, çalıştırılabilir dosyanın içerisindeki gereksiz alanları budayarak dosya boyutunu küçültmeye yaramaktadır. Özellikle statik derleme işlemi sonrasında çok yararlı, olmazsa olmaz bir adımdır. Budama işlemi için şu komutu kullanmak yeterli olacaktır: `strip --strip-all QtUygulamam` Bu işlem sonrasında 21mb olan uygulama boyutumuz 15mb’a kadar düşmüştür.

**QML Optimizasyonu** <br>
QML dosyamız, kullanıcıya gösterilecek ekran olduğu için optimize halde olması, uygulamanın kullanıcıya yansıtılması aşamasını hızlandıracaktır. QML kodlarının tek, büyük bir QML dosyası içinde olması yerine mantıksal olarak parçalara bölünmesi QML kodunun çalışmasını hızlandıracaktır. Biz de bu doğrultuda uygulamamızda bolca olan Label’lara bu işlemi uyguladık. Bu işlem, programı optimize etmese dahi kodun temiz olması açısından yapılması gereken bir işlemdir. Aslında temiz kod, dolaylı yoldan, yazılımın her parçasını etkileyen en önemli optimizasyonlardan biridir.

**QML Lazy Load** <br>
Üzerinde çalıştığımız uygulamanın arayüzü çok kompleks olmadığı için bu özellik bir fark oluşturmamaktadır fakat büyük QML dosyalarında, kullanıcıya ilk gösterilecek arayüzün kodlarının bulunduğu dosya, mümkün olduğunca az kod içermeli. Bu sayede kullanıcı erken yüklenen bu ekranı görürken arka tarafta geri kalan işlemler yapılabilir veya işlemler ihtiyaç duyuldukça başlatılabilir.

**Kaynak Dosyalarını Uygulamaya Gömmek** <br>
.qrc dosyası aracılığı ile projeye eklediğimiz her türlü kaynak, derlenmiş olan programın içine gömülür. Bu uygulamaya hız kazandırır. Bizim programımızda kullandığımız resmi projemize bu şekilde ekledik. Bunun yanında uygulamamız, yazı tipini sistemden almaktaydı. Biz, kaynak kodumuzda ufak değişiklikler yaparak fontu da derlenmiş programımızın içine gömdük.


### <u>Geliştirilebilir</u>

Bu kısımda, sonsuz farklı seçenek olsa da, bahsedilecek olan seçenekler, bize büyük ilerleme sağlayacak ve kabul edilebilir miktarda zaman alacak seçeneklerdir. Aynı zamanda optimizasyon yaparken izlenen yolun nasıl geliştirilebileceği konusunda da tavsiyeler içermektedir.


**Kod** : G1 <br>
**İlgili olduğu kısım** : K1 (start.elf) <br>
**Tahmini kazandıracağı süre** : 0.5sn <br>
**Kullanılabilecek Araç/Yöntem** : Herhangi bir ARM disassembler yeterli olacaktır. <br>
**Açıklama** : start.elf yerine start_cd.elf kullanılabilir. Bunun için start_cd.elf dosyası üzerinde tersine mühendislik yapıp problemi tam olarak tespit etmek gerekiyor. Temel olarak ilk önce start.elf’nin yapısını inceleyerek anlamak gerekiyor. start.elf’de olan, start_cd.elf’de olmayan, bizim ihtiyacımız olan kısımlar tespit edilerek start_cd.elf’e eklenmesi gerekiyor. 


**Kod** : G2 <br>
**İlgili olduğu kısım** : K5 (Qt) <br>
**Tahmini kazandıracağı süre** : 0.9sn <br>
**Kullanılabilecek Araç/Yöntem** : Cache <br>
**Açıklama** : Userspace uygulamaları, ilk açıldıkları zaman yavaş açılırlar. Sonraki açılışlarında  Kernel tarafından Cache’lendikleri için hızlı açılırlar. (bkz. [8][8]) Qt uygulamamızda da aynı durum gözlenmektedir. Eğer Cache’lenmiş uygulamanın Cache’lerini bir şekilde kopyalayıp, Kernel açılışında bunları Kernel’a geçirebilirsek, uygulamamız ilk baştan itibaren hızlı açılır. Kernel kaynak kodunun içindeki “hibernete.c” (bkz. [9][9]) ve “drop_caches.c” (bkz. [10][10]) kaynak kodları bu iş için kullanılabilir. Özellikle Hibernete işlemi, bizim bu yaptığımız işlemi kapsamaktadır.


**Kod** : G3 <br>
**İlgili olduğu kısım** : K3 (kernel.img), K5 (Qt) <br>
**Tahmini kazandıracağı süre** : 1.0sn <br>
**Kullanılabilecek Araç/Yöntem** : Hibernete <br>
**Açıklama** : Hibernete işlemi, sistemin o anki geçici bilgilerini (RAM) sabit hafızaya (bizim için SD kart oluyor) kopyalayarak, sistemi tamamen kapatması olayına denir. Bu işlem, birçok işlemin yapılmaya gerek kalmadan atlanabilmesini sağlamanın yanında Cache’lerin de kaybolmamasını sağladığı için K3 ve K5 aşamalarında büyük bir kazanım getirebilir. Şu an 1.32sn olan K3 ve K5 aşamaları, boot işleminin en fazla zaman alan kısımlarından oldukları için üzerine çalışılmaya değer. Bu işlemin arka planda neler yaptığını iyice bilmeliyiz, tamamen kontrol edebilmeliyiz çünkü bilinçsizce yapılınca stabilite açısından sıkıntı çıkartma ihtimali olabilir. Mesela alınabilecek önlemlerden birisi belli zaman aralıkları ile sistemin tamamen kapatılıp açılması olabilir. Bu sayede RAM ve Cache sıfırlanır. Ayrıca Kernel, Hibernete özelliğini bize zaten sunuyor fakat direkt olarak etkinleştirip kullanılmaya çalışıldığında kullanılamıyor.


**Kod** : G4 <br>
**İlgili olduğu kısım** : K3 (kernel.img), K5 (Qt) <br>
**Tahmini kazandıracağı süre** : - <br>
**Kullanılabilecek Araç/Yöntem** : initramfs, SquashFS <br>
**Açıklama** : SD kartta 2 Partition bulunmaktadır. Boot Partition ve File System Partition. Boot Partition, Raspberry’nin boot olabilmesi için gerekli dosyaları içinde barındırır. Bu nedenle Boot Partition, Raspberry tarafından otomatik okunmaya başlar ve gerekli dosyalar otomatik çalıştırılır. Aslında File System olmadan Kernel, RAM üzerinde çalışır. Biz de kısaca bu durumu kullanmaya çalışabiliriz. Biraz daha detaylara inelim. Şu an uygulamamız File System Partition’a duruyor ve kullanılacağı zaman RAM’e kopyalanarak çalışmaya başlıyor. Yani uygulamamızı çalıştırmak için File System Partition’u bağlamak zorunda kalıyoruz (bu durumun neden olduğu ek işlemler de var) ve bu zaman alan bir işlem. Kernel.img, File System Partition’un kırpılmış, boot süreci için gerekli olan bir kısmının kopyasını içinde barındırıyor. start.elf’in kernel.img’i çalıştırması, kernel.img’in RAM’e kopyalanması demek. Eğer biz uygulamamızı kernel.img’in içine gömersek, start.elf kernel.img’i RAM’e kopyalarken bizim uygulamamız da kopyalanmış olur ve ayrıyetten File System Partition bağlanmasına gerek kalmadan kazanç sağlamış oluruz. Başka bir kazanç da, uygulamamızın direkt RAM üzerinden başlatılacağından dolayı, daha hızlı başlaması olacaktır. Tabiki bunu yapabilmek için uygulamamızın statik derlenmiş olması veya gerekli bütün kütüphanelerin de kernel.img’in içine gömülmesi gerekir. Bu geliştirmenin dezavantajlarından birisi, uygulamamızın RAM’de çalışması halinde Read-Only bir ortamda çalışacak olmasıdır. Tabiki bu problem gerektiğinde aşılabilir.


**Kod** : G5 <br>
**İlgili olduğu kısım** : K3 (kernel.img), K5 (Qt) <br>
**Tahmini kazandıracağı süre** : - <br>
**Kullanılabilecek Araç/Yöntem** : Btrfs, f2fs <br>
**Açıklama** : Bu File System türleri okuma hızı yüksek, okuma ve yazma desteği olan File System türleridir. Hızlı boot için yazma hızından ziyade okuma hızı daha önemlidir. Bu File System’leri denemek için zaman ayırmaya değer.


**Kod** : G100 <br>
**Açıklama** : Optimizasyon işleminin temeli olan debugging işleminin daha planlı yapılması gerekiyor. Optimizasyona başlamada önce debugging planı hazırlamak, debugging için gerekli malzemeleri toplamak, derleme, çalıştırma, geliştirme işlemlerinin otomatize edilmesi, stratejinin belirlenmesi gibi işler için daha çok zaman harcanılması gerekiyor.


**Kod** : G101 <br>
**Açıklama** : Fark edildiyse optimizasyona en düşük seviyeden, yani Raspberry’nin kendini boot etmesinden başlayarak yüksek seviyeye doğru, yani Qt uygulamasının optimizasyonuna doğru ilerledik. Bu yöntem, optimizasyon işleminin temeli olan debugging işini zorlaştırıyor. Bunun yerine optimizasyona Qt uygulamasından başlanırsa daha sağlıklı ilerlenebilir diye düşünüyorum.


### Sonuç

“Normal” ölçümler, Buildroot ile üzerinde ayar yapılmayarak derlenmiş imajın ölçümleridir. Sadece konfigürasyon ayarlarından boot gecikme süresi sıfıra indirilmiştir. Ayrıca Qt uygulamasının kodları değiştirilmemiştir. “Normal” deki K5 ölçümü bir miktar optimizasyon içermektedir.

“ftDev” ise kendi ayarlarımızı yaptığımız imajın verileridir.

|           |**K1** |**K2** |**K3** |**K4** |**K5** |**Toplam** |
|---:       |---    |---    |---    |---    |---    |---        |
|**Normal** |1.25sn |2.12sn |5.12sn |0.12sn |1.56sn |10.17sn    |
|**ftDev**  |1.25sn |0.25sn |0.25sn |0.00sn |1.07sn |2.83sn     |

Not : Ölçümler, cihazın açılışı yüksek hızlı kamera ile çekilerek, daha sonrasında görüntüler yavaşlatılıp izlenerek yapılmıştır.


### Referanslar

**1.** [How the Raspberry Pi boots up](https://thekandyancode.wordpress.com/2013/09/21/how-the-raspberry-pi-boots-up/) <br>
**2.** [Raspberry Pi Firmware](https://github.com/raspberrypi/firmware) <br>
**3.** [Device Tree For Dummies](https://bootlin.com/pub/conferences/2014/elc/petazzoni-device-tree-dummies/petazzoni-device-tree-dummies.pdf) <br>
**4.** [Mergely](http://www.mergely.com/editor) <br>
**5.** [Furkan Tokaç Buildroot](https://gitlab.com/furkantokac/buildroot/blob/ftdev/board/ftdev/rpi3/docs/distro_optimization/fcond04/README.config) <br>
**6.** [RaspberryPi2EGLFS](https://wiki.qt.io/RaspberryPi2EGLFS) <br>
**7.** [Linking to Static Builds of Qt](http://doc.qt.io/QtForDeviceCreation/qtee-static-linking.html) <br>
**8.** [Linux System Administrators Guide / Memory Management / The buffer cache](https://www.tldp.org/LDP/sag/html/buffer-cache.html) <br>
**9.** [Raspberry Pi Boot](http://exileinparadise.com/raspberry_pi_boot) <br>
**10.** [Raspberry / Linux / Bootp](https://github.com/raspberrypi/linux/tree/rpi-4.14.y/arch/arm/boot/bootp) <br>

[1]: https://thekandyancode.wordpress.com/2013/09/21/how-the-raspberry-pi-boots-up/ 
[2]: https://github.com/raspberrypi/firmware 
[3]: https://bootlin.com/pub/conferences/2014/elc/petazzoni-device-tree-dummies/petazzoni-device-tree-dummies.pdf
[4]: http://www.mergely.com/editor
[5]: https://gitlab.com/furkantokac/buildroot/blob/ftdev/board/ftdev/rpi3/docs/distro_optimization/fcond04/README.config
[6]: https://wiki.qt.io/RaspberryPi2EGLFS 
[7]: http://doc.qt.io/QtForDeviceCreation/qtee-static-linking.html 
[8]: https://www.tldp.org/LDP/sag/html/buffer-cache.html 
[9]: http://exileinparadise.com/raspberry_pi_boot 
[10]: https://github.com/raspberrypi/linux/tree/rpi-4.14.y/arch/arm/boot/bootp 
