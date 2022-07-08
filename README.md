# C-digo-clasificador
Código del clasificador de tapitas por color (servomotor, motor paso a paso y sensor de color)
#include <Stepper.h>
#include <Wire.h>
#include "Adafruit_TCS34725.h"
#include <Servo.h>
Servo myservo1;
Servo myservo2;

byte gammatable[256];// our RGB -> eye-recognized gamma color
const int stepsPerRevolution = 512 ;  // change this to fit the number of steps per revolution
int pos = 80;
Adafruit_TCS34725 tcs = Adafruit_TCS34725(TCS34725_INTEGRATIONTIME_50MS, TCS34725_GAIN_4X);

Stepper myStepper(stepsPerRevolution, 8, 10, 9, 11);// initialize the stepper library on pins 8 through 11:

void setup() {
  myservo1.attach(3);
  myservo2.attach(2);
  // set the speed at 15 rpm:
  myStepper.setSpeed(60);
  // initialize the serial port:
  Serial.begin(9600);
 
}

void loop() {
  // step one revolution  in one direction:
  Serial.println();
  myStepper.step(stepsPerRevolution);
  delay(500);
  float red, green, blue;
  tcs.getRGB(&red, &green, &blue);
  red = red-60;
  green = green+30;
  blue = blue+50;
  
  delay(60);   
  Serial.print("R:\t"); Serial.print(int(red)); 
  Serial.print("\tG:\t"); Serial.print(int(green)); 
  Serial.print("\tB:\t"); Serial.print(int(blue));

  for (pos = 80; pos <= 180; pos += 1) { // goes from 0 degrees to 180 degrees
    // in steps of 1 degree
    myservo2.write(pos);              // tell servo to go to position in variable 'pos'
    delay(15);                       // waits 15 ms for the servo to reach the position
  }
  for (pos = 180; pos >= 80; pos -= 1) { // goes from 180 degrees to 0 degrees
    myservo2
    .write(pos);              // tell servo to go to position in variable 'pos'
    delay(15);                       // waits 15 ms for the servo to reach the position
  }
 
  if (red > blue and green > blue and red > 115 and green > 115) {
    myservo1.write(75);
  } else if (green > red and blue > red and green > 115 and red > 115) {
    myservo1.write(50);
  } else if (blue > green and red > green and blue > 115 and red > 115) {
    myservo1.write(0);
  } else if (red > green and red > blue+20 and red >115) {
    myservo1.write(0); 
  } else if (green > red and green > blue and green > 115) {
    myservo1.write(25);
  } else if (blue > red and blue > green and blue > 115) {
    myservo1.write(50);
  } else if (red < 81) {
    myservo1.write(100);    
  } else if (red < 90){
    myservo1.write(150);
  } else if (red < 99){
    myservo1.write(180);
  } else {
    myservo1.write(125);
  }

  Serial.print("\n");
  delay(500);

}
