# Elegoo Arduino Uno: Servos Light Countdown

## Objective

The Arduino Servos Light Countdown project is aimed at establishing a light color switch controlled by a servo motor acting as a clock. The primary focus was to test out the LEDs' on/off commands and switching between each one based on the direction the servo motor was at.

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

This diagram shows what the Arduino complete light countdown should look like. I will list the items I used in the kit to put this together:

- LAFVIN UNO R3 Board
- 830 Point Breadboard
- 5x LED
- 5x Resistors (220)
- Servo Motor (SG90)

<img width="650" height="500" alt="Screenshot2026-09-27213603-upscaled-3x" src="https://github.com/user-attachments/assets/d2d75318-94b1-4914-8f29-ef72c6c2b814" />

Staring off I set up the ground line for the LEDs, placing a black wire shown above in the powered GND pin, then placing the other end in the blue-sided pins, allowing that whole side to be ground. Then, after placing each colored LED, I set one end of the 220 resistors on the blue-sided pins and the other over to the row where the negative ends of LEDs were. 

<img width="570" height="412" alt="660636501-324b3c4c-5bb7-4b94-b874-d1729fd3447d-upscaled-4x" src="https://github.com/user-attachments/assets/9724ddb1-d579-4641-b021-b8724740ac03" />

Next, I get the Servo Motor and attach the pins it needs: a ground pin, a power pin, and a digital pin for the signal, so it will activate once told to do so by uploading the code to the UNO.

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
int servoDelay = 1000;
int servoMax = 180;
int servoMin = 0;
int servoPos = 0;
int timerSeconds = 60;


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

Above shows how the servo and LEDs work in the Arduino programming software. In this part, I will break down each section, explaining how the code works.

```javascript
#include <Servo.h>

Servo myServo;
//LED pins
int redPin = 3;
int bluePin = 5;
int greenPin = 6;
int yellowPin = 10;
int whitePin = 11;
```

Here in the first line shows the inclusion of the Servo library, which allows my Arduino IDE to use Servo commands, like the next line, where I make an object for the servo so I can tell it what to do. Based on the comment, we can see each colored LEDs getting stated into a variable based on the digital power PIN each one resides in; this will be used later.

```javascript
//Serbo clock
int servoDelay = 1000;
int servoMax = 180;
int servoMin = 0;
int servoPos = 0;
int timerSeconds = 60;
```

This is where we start configuring the motor. The ``` servoDelay ``` is the number of times to stop between ticks (for consistency, I will refer to each movement as a tick, and a full 180 rotation as a full tick). 

Delays in JavaScript are in milliseconds, so 1000 milliseconds == 1 second. The maximum amount the motor can rotate is 180 degrees; we can allow this full tick by using ```servoMax``` and setting it to 180. ```servoMin``` is, of course, the minimum the motor can be set to; to get a full tick, I will set it to 0. ```servoPos``` is meant to check the position the motor is at and will restart based on the maximum degrees from the ```servoMax``` variable. 

Now the full tick should only take one minute to reach the maximum degrees, neither more nor less than, so I will make a variable ```timerSeconds``` so it can reach a full tick in 60 seconds. All of these variables will be used to help the functions of the motor and will be used in the void loop.

```javascript
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
```

```myServo.attach(13);``` will connect the servo to the UNO on pin 13. The ```pinMode``` for each color will, of course, be set to OUTPUT for each interaction from the UNO. The ```LOW``` part in ```digitalWrite``` will tell the UNO not to put power onto the pins yet; they will turn on in the for loop.

```
void loop() {
  // put your main code here, to run repeatedly:
  for (servoPos = servoMin; servoPos <= servoMax; servoPos+= (servoMax/timerSeconds)){
    myServo.write(servoPos);
    delay(servoDelay);
```

I will explain the for loop as best as possible. For reminders, ```servoPos``` tells us the position the motor is at after each tick. This variable is extremely important because it is used to give us a starting point, when it will stop and restart the motor, how to calculate the position of the motor, and finally when to turn on the LEDs. 

For the starting point, I will make sure ```servoPos``` be equal to the minimum the motor should start with, which is 0 so I can get a full 180 degrees, The condition will tell ```servoPos``` to continue increasing till it reaches a number that is grater then or equal to the maximum position it can the motor can rotate, which would be 180, and we can set this by using the ```servoMax``` variable to make this condition, lastly if it is less the maximum position, it will keep updating to the next amount of numbers, but this is where things get complicated, what is the next amount? To get this, we would have to use the maximum position it can be, ```servoMax``` and the time each tick should take, ```timerSeconds```. 

Each tick is 1000 milliseconds, which is 1 second. We need this to tell how much should change each second. To get this, we will divide ```servoMax```by ```timerSeconds``` to get the quotient of the range of motion of the motor by the number of seconds it should take; with that, we have the next value of the ```servoPos``` position.

Now, finally they are two more things to add before we start with the if statements: first, we need to add the ```servoPos``` variable to the ```myServo``` object so the motor can move to the position; second, we need to add the 1-second delay between each tick. This can be done by adding the ```servoDelay``` variable inside the ```delay()``` statement.

```
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

