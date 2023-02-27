# Animatronic

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

[![Myndband](https://linksharing.samsungcloud.com/ls/public/v1/links/1676903574644LjVbwH3/contents/7bbf0620b12b11ed89ba8eddef23ce47/resized/760?signature=KBNoHMgUK6vfyaIajYqss8v8IObbKY2LR_eDRbY0UEj9u40vlmKVPd2j03zaSXrLit0NhUNfBjsY6CZ5AjRLaCmGzWpEefkPp7JAql8l_K1nEBm4H-5d476BLS736vfvVwv7mGFHmMjdMUKOD2DfOg&storageType=file)](https://linksharing.samsungcloud.com/contents/view?contentsToken=1676903574644LjVbwH3&currentIndex=1&linkUrlVersion=V1)

[![Myndband](https://linksharing.samsungcloud.com/ls/public/v1/links/1676904834867LucppPn/contents/6b496ee0b12e11ed93cfb29167985735/resized/760?signature=KBNoHMgUK6vfyaIajYqss8v8IObbKY2LR_eDRbY0UEj9u40vlmKVPd2j03zaSXrLit0NhUNfBjsY6CZ5AjRLaCmGzWpEefkPp7JAql8l_K1nEBm4H-5d476BLS736vfvVwv7mGFHmMjdMUKOD2DfOg&storageType=file)](https://linksharing.samsungcloud.com/contents/view?contentsToken=1676904834867LucppPn&currentIndex=1&linkUrlVersion=V1)


**Emil Sharifi, Lukas Grigaliunas, Lúkas Aron Speight**
