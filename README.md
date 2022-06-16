# C-digo-clasificador
Código del clasificador de tapitas por color (servomotor, motor paso a paso y sensor de color)
#include <Stepper.h>
#include <Wire.h>
#include "Adafruit_TCS34725.h"

byte gammatable[256];// our RGB -> eye-recognized gamma color
const int stepsPerRevolution = 512 ;  // change this to fit the number of steps per revolution

Adafruit_TCS34725 tcs = Adafruit_TCS34725(TCS34725_INTEGRATIONTIME_50MS, TCS34725_GAIN_4X);

Stepper myStepper(stepsPerRevolution, 8, 10, 9, 11);// initialize the stepper library on pins 8 through 11:

void setup() {

  // set the speed at 15 rpm:
  myStepper.setSpeed(60);
  // initialize the serial port:
  Serial.begin(9600);
  if (tcs.begin()) {
    //Serial.println("Found sensor");
  } else {
    Serial.println("No TCS34725 found ... check your connections");
    while (1); // halt!
  }
}

void loop() {
  // step one revolution  in one direction:
  Serial.println();
  myStepper.step(stepsPerRevolution);
  delay(500);
  float red, green, blue;
  
  delay(60); 

  tcs.getRGB(&red, &green, &blue);
  
  Serial.print("R:\t"); Serial.print(int(red)); 
  Serial.print("\tG:\t"); Serial.print(int(green)); 
  Serial.print("\tB:\t"); Serial.print(int(blue));

  Serial.print("\n");
  delay(1000);

}
