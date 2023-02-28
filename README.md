# Animatronic

Við notuðum 2 servó mótora. Einn fyrir kjálkann og einn fyrir líkamshreyfinguna. Notuð LED fyrir augu og HC-SRO4 til að greina eitthvað fyrir framan animatronic. Hátalararnir og mp3 spilarinn virkuðu ekki og kennarinn lét okkur sleppa mp3 spilaranum og hátölurunum.

Hann er 48 cm hátt, sýrka 49cm breidd og 42 cm lengd

## Kóðan

```python
/* Servo mótor.
 * Brúnn tengist í GND, rauður tengist í 5V og 
 * appelsínugulur í pinnann sem stjórnar snúningnum
 */

#include <Servo.h> // Sambærilegt og import í python
#include "tdelay.h"

Servo motor1; // bý til tilvik af Servo klasanum
Servo motor2;

int motor1_pinni = 9; 
int motor2_pinni = 8;// pinninn sem ég nota til að stjórna mótornum
const int ECHO = 2; 
const int TRIG = 3; 
#define MAX_DISTANCE 300
#define MIN_DISTANCE 100

int motor1_stefnur[] = {50,120}; 
int motor1_stefnu_fjoldi = 2; 
int motor1_stefnu_teljari = 0;
int motor2_stefnur[] = {0,90}; 
int motor2_stefnu_fjoldi = 2; 
int motor2_stefnu_teljari = 0; // breytan heldur utan um í hvaða stefnu mótorinn á að benda
TDelay motor1_bid(1000);
TDelay motor2_bid(300); 
int fjarlaegd(); 
int dist = 0;


void setup() {

  motor1.attach(motor1_pinni); // segi servo tilvikinu hvaða pinna á að nota
  motor2.attach(motor2_pinni);
  motor1.write(motor1_stefnur[motor1_stefnu_teljari]);
  motor2.write(motor2_stefnur[motor2_stefnu_teljari]);
  Serial.begin(9600); 

  pinMode(TRIG,OUTPUT);
  pinMode(ECHO,INPUT);


}

void loop() {
  dist = fjarlaegd();
  //Serial.println(dist);
  if(dist < 0)
    dist = 0;
  if(dist < 80 && dist != 0) {

      if(motor1_bid.timiLidinn()) {
    // uppfæra stefnu_teljara breytuna, modulus notað til að talan verði
    // aldrei hærri en fjöldi stefnanna sem eru í listanum
        motor1_stefnu_teljari = (motor1_stefnu_teljari + 1) % motor1_stefnu_fjoldi;
    // veljum svo rétta stefnu úr listanum
        motor1.write(motor1_stefnur[motor1_stefnu_teljari]);

      }
      if(motor2_bid.timiLidinn()) {
    // uppfæra stefnu_teljara breytuna, modulus notað til að talan verði
    // aldrei hærri en fjöldi stefnanna sem eru í listanum
        motor2_stefnu_teljari = (motor2_stefnu_teljari + 1) % motor2_stefnu_fjoldi;
    // veljum svo rétta stefnu úr listanum
        motor2.write(motor2_stefnur[motor2_stefnu_teljari]);

      }

  }
  Serial.print("Fjarlaegd: ");
  Serial.println(fjarlaegd());
}
int fjarlaegd() {
    // sendir út púlsa
    digitalWrite(TRIG,LOW);
    delayMicroseconds(2); // of stutt delay til að skipta máli
    digitalWrite(TRIG,HIGH);
    delayMicroseconds(10); // of stutt delay til að skipta máli
    digitalWrite(TRIG,LOW);

    // mælt hvað púlsarnir voru lengi að berast til baka

    return (int)pulseIn(ECHO,HIGH)/58.0; // deilt með 58 til að breyta í cm
}
```

### Myndir og myndband af mekatrónik

### Haus

[![Myndband](https://i.ytimg.com/vi/jkB-AlOwPUE/hq2.jpg?sqp=-oaymwE9CNACELwBSFryq4qpAy8IARUAAAAAGAAlAADIQj0AgKJDeAHwAQH4Ac4FgAKACooCDAgAEAEYSiBlKEcwDw==&rs=AOn4CLBIq4EuMFZlA7SF92BV9sHLipYARg)](https://youtube.com/shorts/jkB-AlOwPUE?feature=share)

### Hreyfing

[![Myndband](https://i9.ytimg.com/vi/M5PZUiJe6Es/mqdefault.jpg?sqp=CNDf9p8G-oaymwEoCMACELQB8quKqQMcGADwAQH4AYwCgALgA4oCDAgAEAEYZSBMKD4wDw==&rs=AOn4CLDnlBSueFqayNqS2RCTTPgRxc0X3A)](https://www.youtube.com/shorts/M5PZUiJe6Es)

### Lokaniðurstaða

[![Myndband](https://i9.ytimg.com/vi/p6S7PY_sBiY/mqdefault.jpg?sqp=CNDf9p8G-oaymwEoCMACELQB8quKqQMcGADwAQH4AYwCgALgA4oCDAgAEAEYciBUKD8wDw==&rs=AOn4CLAgXghaWrP8WoIbmo_QBfD5Yf7P8g)](https://www.youtube.com/shorts/p6S7PY_sBiY)

### Mynd 

![Mynd](https://cdn.discordapp.com/attachments/1062375783718469663/1080034786132897802/20230220_120658.jpg)

**Emil Sharifi, Lukas Grigaliunas, Lúkas Aron Speight**
