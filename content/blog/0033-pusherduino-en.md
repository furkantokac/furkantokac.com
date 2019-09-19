---
title: "Pusherduino : An Arduino Game"
date: "2016-09-14T15:15:24+03:00"
thumbnail: "/img/0032-pusherduino.png"
categories: ["Elektronik|Electronics"]
tags: ["C", "Arduino", "English", "Electronic"]
url: "pusherduino-en"
summary: "Pusherduino is an Arduino based, simple and excited board game. Follow the comments in source code for more info. There are 4 buttons and each button has a led. When the game start, one led is turned on. Gamer should push the button of the led in the given time interval."
---

{{< goTrPost url="/pusherduino" >}} <br>

Project Link : [https://circuits.io/circuits/2710731-pusher/](https://circuits.io/circuits/2710731-pusher/) </br>
Github Link : [https://github.com/furkantokac/Pusherduino/](https://github.com/furkantokac/Pusherduino/)

## Video

{{< youtube hvHljwiRgoY >}} </br>


## Description

Pusherduino is an Arduino based, simple and excited board game. Follow the comments in source code for more info.

Basic explanation of the game : There are 4 buttons and each button has a led. When the game start, one led is turned on. Gamer should push the button of the led in the given time interval. If gamer can push the correct button, s/he will make a score and delayTime will decrease. If gamer push the wrong button or cannot push the button in the given time interval, s/he will lose a life. If gamer makes a combo, s/he will gain a life.

Note : If you will build the game on your own Arduino, do not forget to put resistors for LEDs.


## Source Code

{{< highlight c >}}
/*GAMEL LOGIC
There are 4 buttons and each button has a led. When the game start, one led is turned on.
Gamer should push the button of the led in the given time interval. If gamer can push the
correct button, s/he will make a score and delayTime will decrease. If gamer push the wrong
button or cannot push the button in the given time interval, s/he will lose a life. If gamer
makes a combo, s/he will gain a life.
*/

#define LEDS 4
/*You can easily add extra leds&buttons by changing this definition and adding the ports
to LEDpin and BTNpin.*/

// The led of BTNpin[0] is LEDpin[0].
const byte LEDpin[LEDS] = {9,10,11,12};
const byte BTNpin[LEDS] = {2,3,4,5};
const byte losePointPIN = 7; // This led will be flash when losing point
const byte winPointPIN  = 8; // This led will be flash when losing point

unsigned long score=0;
unsigned int scoreChangeSpeed=1; // this will be added to score

unsigned int delayTime=5000; // Game will be harder when delayTime is decreased
unsigned int delayTimeChangeSpeed=150;

byte life=10;

byte flag=1;

// if currentCombo > limitCombo, life += 1
// if gamer make a mistake, currentCombo will be 0
byte currentCombo=0;
byte limitCombo=20;

void setup()
{
    byte i = 0;
    
    // Defining BTNpins and LEDpins
    for(i =0; i < LEDS; i++)
    {
        pinMode(LEDpin[i], OUTPUT);
        pinMode(BTNpin[i], INPUT);
        digitalWrite(BTNpin[i], HIGH); // To use buttons without extra supply
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

        updateGUI();
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

// A round starts with this funtion. If the function returns true, gamer win the round. If the function returns false, gamer lose the round.
boolean play()
{
    // Choose a random led to turn on
    byte target = random(0,LEDS);
    digitalWrite(LEDpin[target], HIGH);
  
    /*currentTime millis() is 100 //// delayTime=10 //// 100+delayTime=110 //// while(millis < 110) is waiting for the millis become 110*/
    unsigned long finishAt = millis() + delayTime;
    while(millis() < finishAt)
    {
        if(digitalRead(BTNpin[target]) == LOW) 
        {
            digitalWrite(LEDpin[target], LOW);
            //Serial.println("Correct Button!!!+++++++++++");
            return 1;
        }
    
        if(wrongButton(target))
        {
            //Serial.println("Wrong Button!!!----------");
            return 0;
        }
    }
  
    digitalWrite(LEDpin[target], LOW);
    //Serial.println("Timeout!!!-------");
    return 0;
}

// Turn on or off the leds
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
        if( limitCombo>3 ) // limit of the combo cannot be less than 3
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
