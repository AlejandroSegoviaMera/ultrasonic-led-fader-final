////////////////////////////////////////////////////////////////////////////////////////
// OVERVIEW
////////////////////////////////////////////////////////////////////////////////////////
/*
DESCRIPTION: This code is for hacking and testing only.

AUTHOR: Alejandro Segovia Mera

COMMENTS: 


HISTORY: 

*/


////////////////////////////////////////////////////////////////////////////////////////
// INCLUDES 
////////////////////////////////////////////////////////////////////////////////////////

#include <Arduino.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <math.h>
#include <avr/pgmspace.h> 

using namespace std;

////////////////////////////////////////////////////////////////////////////////////////
// STRUCTS AND CLASSES
////////////////////////////////////////////////////////////////////////////////////////


////////////////////////////////////////////////////////////////////////////////////////
// PROTOTYPES
////////////////////////////////////////////////////////////////////////////////////////


////////////////////////////////////////////////////////////////////////////////////////
// DEFINES AND MACROS
////////////////////////////////////////////////////////////////////////////////////////


////////////////////////////////////////////////////////////////////////////////////////
// GLOBALS
////////////////////////////////////////////////////////////////////////////////////////

// Declare pins as globals


const int trigPin = 9;                                 // For HC-SR04
const int echoPin = 10;                                // For HC-SR04

const int LEDPin = 6;                                  // For THE LED
const int fadeAmount = 5;                              // how many points to fade the LED by

float duration, raw_distance, filter_distance;         // variables used with echoPin

float targetBrightness, currentBrightness = 0;         // variables declared for the brightness

float previous_dist = 0, clamped_distance = 0;         // variables declared for the distances

////////////////////////////////////////////////////////////////////////////////////////
// FUNCTIONS, API, METHODS
////////////////////////////////////////////////////////////////////////////////////////




///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

void setup() 
{

pinMode(trigPin, OUTPUT);
pinMode(echoPin, INPUT);
pinMode(LEDPin, OUTPUT);

Serial.begin(115200);
Serial.print("The state of echoPin is: ");
Serial.println(digitalRead(echoPin));


} // end setup

///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

void loop() 
{


//define the states of the Trigger
digitalWrite(trigPin, LOW);
delayMicroseconds(2);
digitalWrite(trigPin, HIGH);
delayMicroseconds(10);
digitalWrite(trigPin, LOW);

duration = pulseIn(echoPin, HIGH);
raw_distance = (duration * 0.0343)/2;   // speed of sound = 0.0343cm/µs


filter_distance = 0.3 * raw_distance + (1 - 0.3) * previous_dist;   // using Smooth exponential A_n = alpha * M + (1 - alpha) * A_(n-1)
previous_dist  = filter_distance;

Serial.print("Distance in cm: ");
Serial.println(previous_dist);

delay(20);

// Clamp the filtered distance

if(previous_dist <= 10)
  {
    clamped_distance = 10;
  } //end if

else if (previous_dist >= 20)
  {
    clamped_distance = 20;
  } // end else if

else
  {
    clamped_distance = previous_dist;
  } // end else



// map final LED brightness to object distance
targetBrightness = map(clamped_distance, 10,20,255,0);


// Slow fade effect
if(currentBrightness < targetBrightness)
  {
    currentBrightness += fadeAmount;
  } //end if

else if (currentBrightness > targetBrightness)
  {
    currentBrightness -= fadeAmount;
  } //end else if

else
  {
  // do nothing
  }

currentBrightness = constrain(currentBrightness, 0, 255);


// send PWM to the LED
analogWrite(LEDPin, currentBrightness);


// wait for 30 milliseconds to see the dimming effect
delay(30);

} // end loop

////////////////////////////////////////////////////////////////////////////////////////
