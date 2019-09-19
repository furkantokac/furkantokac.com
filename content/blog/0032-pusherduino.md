---
title: "Pusherduino : Bir Arduino Oyunu"
date: "2016-09-14T15:14:24+03:00"
thumbnail: "/img/0032-pusherduino.png"
categories: ["Elektronik|Electronics"]
tags: ["C", "Arduino", "Elektronik"]
url: "pusherduino"
summary: "Pusherduino Arduino ile yapılmış, basit ve eğlenceli bir oyundur. Kod hakkında bilgiyi koddaki yorumlardan sağlayabilirsiniz. 4 buton ve 4 led var. (Basit bir şekilde arttırılabilir) Her buton bir ledi karşılamakta. Oyun başladığında bir led yanacak."
---

{{< goEnPost url="/pusherduino-en" >}} <br>

Proje linki : [https://circuits.io/circuits/2710731-pusher/](https://circuits.io/circuits/2710731-pusher/) </br>
Github Linki : [https://github.com/furkantokac/Pusherduino/](https://github.com/furkantokac/Pusherduino/) 

## Video

{{< youtube hvHljwiRgoY >}} </br>


## Açıklama

Pusherduino (biliyorum isim çok orjinal :D ) Arduino ile yapılmış, basit ve eğlenceli bir oyundur. Kod hakkında bilgiyi koddaki yorumlardan sağlayabilirsiniz.

Oyunu kısaca açıklayacak olursak : 4 buton ve 4 led var. (Basit bir şekilde arttırılabilir) Her buton bir ledi karşılamakta. Oyun başladığında bir led yanacak. Oyuncu, yanan ledin altındaki butona, verilen zaman aralığında basması gerekiyor. Doğru butona basabilirse "scoru"u yükselecek ve "delayTime" azalacak. "delayTime" butona basmak için ne kadar zamanımızın olduğunu tutan değişken. Eğer yanlış butona tıklarsa veya verilen zaman aralığında doğru butona basamazsa can azalacak. Combo yaparsa 1 can kazanacak.

Not : Oyunu kendi Arduino'nuz üzerinde deneyecekseniz ledler için direnç kullanmayı unutmayın.


## Program Kodu

{{< highlight c >}}
/*OYUNUN GENEL MANTIGI
Hazirda 4 buton ve 4 led var. Her buton bir ledi karsiliyor. Oyun basladiginda bir led yanacak. 
Oyuncu, yanan ledin altindaki butona, verilen zaman araliginda basmasi gerekiyor. Dogru butona
tiklarsa score'u yukselecek ve delayTime daha azalacak. Eger yanlis butona tiklarsa veya 
verilen zaman araliginda dogru butona tiklayamazsa can azalacak. Combo yaparsa 1 can kazanacak.
*/

#define LEDS 4
/*Ne kadar led oldugunu yazacagiz
Yalnizca yukaridaki sayiyi degistirerek oyununuza led ekleyebilir veya cikartabilirsiniz
Ekleme yapacaksaniz LEDpin ve BTNpin lere yeni eklenen parcalarin pinlerini girmelisiniz*/

// LEDpin ve BTNpin deki pin numaralari sirasiyla yazilmistir. LEDpin[0]'in butonu BTNpin[0]'dir.
const byte LEDpin[LEDS] = {9,10,11,12};  // Oyun LED'lerinin bulundugu pinler
const byte BTNpin[LEDS] = {2,3,4,5};     // Oyun butonlarinin bulundugu pinler
const byte losePointPIN = 7;             // Puan kaybedildiginde yanacak olan ledin pini
const byte winPointPIN  = 8;             // Puan kazanildiginda yanacak olan mavi ledin pini

unsigned long score=0;
unsigned int scoreChangeSpeed=1; // Bu degiskendeki deger score'a eklenecek

unsigned int delayTime=5000; // Dusuk delayTime, daha zor oyun anlamina geliyor
unsigned int delayTimeChangeSpeed=150;

byte life=10; // 10 cani var

byte flag=1;

// eger currentCombo limitCombo uzerine cikarsa can kazanacak
// bir kere yanlista currentCombo sifirlanir
byte currentCombo=0;
byte limitCombo=20;

void setup()
{
    byte i = 0;
    
    //BTNpin ve LEDpinlerin tanimlamalari
    for(i =0; i < LEDS; i++)
    {
        pinMode(LEDpin[i], OUTPUT);
        pinMode(BTNpin[i], INPUT);
        digitalWrite(BTNpin[i], HIGH);  // Butonlarin pinleri HIGH yaptik ki devre tamamlansin
    }
    pinMode(losePointPIN, OUTPUT);
    pinMode(winPointPIN, OUTPUT);
    
    Serial.begin(9600);
    Serial.print("\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n");
}

void loop()
{
    if( life > 0 )
    {
        if(play())
            winPoint();
        else
            losePoint();

        updateGUI(); // Oyuncuya canini, scoreunu vs. goster
        setLedStates(LOW);
        delay(150);
    }
    else//( life<=0 )
    {
        if( flag==1 )
        {
            //life=10;
            Serial.println("***GAME OVER***");
            Serial.print("***************Your score is "); Serial.println(score);
            Serial.print("Press reset button on arduino to start");
            Serial.print("\n\n\n\n\n\n\n\n");
            flag=0;
        }
        setLedStates(HIGH);
        delay(150);
        setLedStates(LOW);
        delay(150);
    }
}

void updateGUI()
{
    Serial.print("Score : "); Serial.println(score);
    Serial.print("Delay : "); Serial.println(delayTime);
    Serial.print(" Life : "); Serial.println(life);
    Serial.print("Combo : "); Serial.println(currentCombo);
    
    Serial.println(" ");
}

// Oyunu bu fonksiyon ile baslatiyoruz. false dondururse puan kaybettik, true dondururse puan kazandik
boolean play()
{
    //Siradaki turda RASTGELE yanacak olan led burada secilecek
    byte target = random(0,LEDS);
    digitalWrite(LEDpin[target], HIGH);
  
    /*kullanicinin ledi yanan butona basacagi zamani ayarlayan kod
    millis() fonskiyonu arduinonun o anki zamanini dondurur. Biz o anki zamanin ustune delayTime sayisini ekleyip, millis() sayisinin yeni sayiya esit olmasini bekliyoruz
    ORNEK: su anki zaman 100 //// delayTime=10 //// 100+delayTime=110 //// while ile de while(oAnkiZaman < 110) o anki zamanin 110 olmasini bekliyoruz*/
    unsigned long finishAt = millis() + delayTime;
    while(millis() < finishAt)
    {
        if(digitalRead(BTNpin[target]) == LOW) 
        {
            digitalWrite(LEDpin[target], LOW);
            //Serial.println("DOGRU Button!!!+++++++++++");
            return 1;
        }
    
        if(wrongButton(target))
        {
            //Serial.println("YANLIS Button!!!----------");
            return 0;
        }
    }
  
    digitalWrite(LEDpin[target], LOW);
    //Serial.println("ZAMAN GECTI!!!-------");
    return 0;
}

// Ledleri ac yada kapat
void setLedStates(byte state)
{
    int i=0;
    for(i=0; i < LEDS; i++)
        digitalWrite(LEDpin[i], state);
}

void winPoint()
{
    onOff(winPointPIN,5);
    delayTime -= delayTimeChangeSpeed;
    if ( delayTime <= 300 )
        delayTime = 300;
    
    currentCombo++;
    score+=scoreChangeSpeed;
    if( currentCombo>limitCombo )
    {
        Serial.println("*****Earned life !******");
        life++;
        currentCombo=0;
        score+=5;
        if( limitCombo>3 ) // limitCombo 3'ten kucuk olamaz
            limitCombo--;
    }
    if( delayTimeChangeSpeed>50 )
        delayTimeChangeSpeed -= 1;
}

void losePoint()
{
    onOff(losePointPIN,5);
    delayTime += delayTimeChangeSpeed;
    delayTimeChangeSpeed += 1;
    currentCombo=0;
    limitCombo=20;
    life--;
}

void onOff(byte ledPIN, byte repeating)
{
    byte c=0;
    while(c < repeating)
    {
        digitalWrite(ledPIN, HIGH);
        delay(30);
        digitalWrite(ledPIN, LOW);
        delay(30);
        c++;
    }
}

boolean wrongButton(byte currentButton)
{
    byte i = 0;
    for(i=0; i < LEDS; i++)
        if(currentButton != i && digitalRead(BTNpin[i]) == LOW)
            return 1;
    return 0;
}
{{< /highlight >}}
