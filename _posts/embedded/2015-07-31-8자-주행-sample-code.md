---
title: "8자 주행 sample code"
excerpt_separator: "<!--more-->"
date: 2015-07-31 20:01:32 +0900
categories:
  - embedded
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

#define SPEED 80  // you can use from 0 ~ 256

#define Motor\_right\_0 3

#define Motor\_right\_1 5

#define Motor\_left\_0 6

#define Motor\_left\_1 9

enum direction{

RIGHT = 0,

STRAIGHT,

LEFT,

STOP,

BACK,

};

void setup()

{

pinMode(Motor\_right\_0, OUTPUT);

pinMode(Motor\_right\_1, OUTPUT);

pinMode(Motor\_left\_0, OUTPUT);

pinMode(Motor\_left\_1, OUTPUT);

Serial.begin(9600);

}

void turn(int d)

{

if(d == RIGHT)

{

analogWrite(Motor\_right\_1, SPEED/2);

digitalWrite(Motor\_right\_0, LOW);

analogWrite(Motor\_left\_0, SPEED);

digitalWrite(Motor\_left\_1, LOW);

}

else if ( d == LEFT)

{

analogWrite(Motor\_right\_1, SPEED);

digitalWrite(Motor\_right\_0, LOW);

analogWrite(Motor\_left\_0, SPEED/2);

digitalWrite(Motor\_left\_1, LOW);

}

else if (d == STRAIGHT)

{  //for STRAIGHT

analogWrite(Motor\_right\_1, SPEED);

digitalWrite(Motor\_right\_0, LOW);

analogWrite(Motor\_left\_0, SPEED);

digitalWrite(Motor\_left\_1, LOW);

}

}

void loop()

{

turn(STRAIGHT);

delay(1000);

turn(LEFT);

delay(1000);

turn(STRAIGHT);

delay(1000);

turn(RIGHT);

delay(1000);

delay(100);

}