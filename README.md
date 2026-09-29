# Elegoo Arduino Uno: Servos Light Countdown

## Objective

The Arduino Servos Light Countdown project is aimed to establish a light color switch controlled by a servo motor acting as a clock. The primary focus was to test out the LEDs on/off commands and switching between each one base on the direction the servo motor was present at.

### Skills Learned

- Advanced understanding of SIEM concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used

- Open-source development environment tools for writing and uploading code to the Arduino.
- An Arduino programming kit to construct the project.

## Steps

### Arduino Diagram

<img width="750" height="600" alt="image" src="https://github.com/user-attachments/assets/d47f9621-67b7-4d1e-8e55-cbdfb66059e1" />

This diagram shows how the Arduino complete light countdown should look like, I will list the items I used in the kit to put this together:

- LAFVIN UNO R3 Board
- 830 PointBreadboard
- 5x LED
- 5x Resistors (220)
- Servo Motor (SG90)

<img width="650" height="500" alt="Screenshot2026-09-27213603-upscaled-3x" src="https://github.com/user-attachments/assets/d2d75318-94b1-4914-8f29-ef72c6c2b814" />

Staring off I set up the ground line for the LEDs, placing a black wire shown above in the powered GND pin, then placing the other end in the blue sided pins, allowing that whole side to be ground, then after placing each colored LED, I set one end of the 220 resistors on the blue sided pins and the other over to the row where the negative ends of LEDs were at. 

<img width="570" height="412" alt="660636501-324b3c4c-5bb7-4b94-b874-d1729fd3447d-upscaled-4x" src="https://github.com/user-attachments/assets/9724ddb1-d579-4641-b021-b8724740ac03" />

Next I get the Servo Motor and attach pin it needs, a ground pin, a power pin, then a digital power pin for the signal, so it will activate once told to from uploading the code into the UNO.

### Arduino Code

```javascript
#include <Servo.h>

Servo myServo;
//LED pins
int redPin = 3;
int bluePin = 5;
int greenPin = 6;
int yellowPin = 10;
int whitePin = 11;


//Serbo clock
int servoDelay=1000;
int servoMax=180;
int servoMin=0;
int servoPos=0;
int timerSeconds=60;


void setup() {
  // put your setup code here, to run once:
  myServo.attach(13);

  pinMode (redPin, OUTPUT);
  pinMode (bluePin, OUTPUT);
  pinMode (greenPin, OUTPUT);
  pinMode (yellowPin, OUTPUT);
  pinMode (whitePin, OUTPUT);

  digitalWrite(redPin, LOW);
  digitalWrite(bluePin, LOW);
  digitalWrite(greenPin, LOW);
  digitalWrite(yellowPin, LOW);
  digitalWrite(whitePin, LOW);
}

void loop() {
  // put your main code here, to run repeatedly:
  for (servoPos = servoMin; servoPos <= servoMax; servoPos+= (servoMax/timerSeconds)){
    myServo.write(servoPos);
    delay(servoDelay);


    if(servoPos == 36){
      digitalWrite(redPin, HIGH);
    }
      else if(servoPos == 72){
        digitalWrite(redPin, LOW);
        digitalWrite(bluePin, HIGH);
     }
        else if(servoPos == 108){
          digitalWrite(bluePin, LOW);
          digitalWrite(greenPin, HIGH);
        }

          else if(servoPos == 144){
            digitalWrite(greenPin, LOW);
            digitalWrite(yellowPin, HIGH);
          }

            else if(servoPos == 180) {
              digitalWrite(yellowPin, LOW);
              digitalWrite(whitePin, HIGH);
            }

            else if(servoPos == 0){
              digitalWrite(redPin, LOW);
              digitalWrite(bluePin, LOW);
              digitalWrite(greenPin, LOW);
              digitalWrite(yellowPin, LOW);
              digitalWrite(whitePin, LOW);
            }

    }
}
```

Above shows hows the servo and LEDs work from the Arduino programming software, in this part I will breakdown each section, explaining how the code works.

```
#include <Servo.h>

Servo myServo;
//LED pins
int redPin = 3;
int bluePin = 5;
int greenPin = 6;
int yellowPin = 10;
int whitePin = 11;
```

Here in the first line shows the adding of the servo from the library, which would allow my Arduino IDE to use Servo commands like the next line, where I make an object for the sevro so I can tell it what to do. base on the comment, we can see each colored LEDs getting stated into a variable based on the digital power pin number each one reside in, this will be used later.

```
//Serbo clock
int servoDelay=1000;
int servoMax=180;
int servoMin=0;
int servoPos=0;
int timerSeconds=60;
```
