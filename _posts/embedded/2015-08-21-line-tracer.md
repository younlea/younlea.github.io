---
title: "line tracer"
excerpt_separator: "<!--more-->"
date: 2015-08-21 01:14:34 +0900
categories:
  - embedded
tags:
  - develop_env
  - embedded
  - 임베디드

toc : true
toc_sticky : true
---

line tracer sensor check

----------------------------------------------------------------------

#define LEFT 10

#define CENTER 11

#define RIGHT 12

void setup()

{

pinMode(LEFT, INPUT);

pinMode(CENTER, INPUT);

pinMode(RIGHT, INPUT);

Serial.begin(9600);

}

void loop()

{

int left,center,right;

left= digitalRead(LEFT);

center= digitalRead(CENTER);

right= digitalRead(RIGHT);

Serial.print("LEFT ");Serial.print(left);

Serial.print(" CENTER ");Serial.print(center);

Serial.print(" RIGHT ");Serial.print(right);

Serial.println(".");

delay(1000);

}

---------------------------------------------------------------------

result

---------------------------------------------------------------------

LEFT 1 CENTER 1 RIGHT 1.

LEFT 1 CENTER 1 RIGHT 1.

LEFT 1 CENTER 1 RIGHT 1.

LEFT 1 CENTER 0 RIGHT 1.

LEFT 1 CENTER 0 RIGHT 1.

LEFT 1 CENTER 1 RIGHT 0.

LEFT 1 CENTER 1 RIGHT 0.

LEFT 0 CENTER 1 RIGHT 1.

---------------------------------------------------------------------

sample code : 오른쪽 왼쪽 정면 check 해서 라인을 중간으로 만들어서 계속 가는 코드.

-----------------------------------------------------------------------

#define SPEED 80  // you can use from 0 ~ 256

//we can use below port number, becuase they can support PWM signal.

#define Motor\_right\_0 3

#define Motor\_right\_1 5

#define Motor\_left\_0 6

#define Motor\_left\_1 9

#define LEFT\_P 10

#define CENTER\_P 11

#define RIGHT\_P 12

enum direction{

RIGHT = 0,

STRAIGHT,

LEFT,

STOP,

BACK,

};

void setup()

{

pinMode(LEFT\_P, INPUT);

pinMode(CENTER\_P, INPUT);

pinMode(RIGHT\_P, INPUT);

pinMode(Motor\_right\_0, OUTPUT);

pinMode(Motor\_right\_1, OUTPUT);

pinMode(Motor\_left\_0, OUTPUT);

pinMode(Motor\_left\_1, OUTPUT);

Serial.begin(9600);

}

void turn(int d)

{

#if 0

if(d == RIGHT)

{

analogWrite(Motor\_right\_0, SPEED);

digitalWrite(Motor\_right\_1, LOW);

analogWrite(Motor\_left\_0, SPEED);

digitalWrite(Motor\_left\_1, LOW);

}

else if ( d == LEFT)

{

analogWrite(Motor\_right\_1, SPEED);

digitalWrite(Motor\_right\_0, LOW);

analogWrite(Motor\_left\_1, SPEED);

digitalWrite(Motor\_left\_0, LOW);

}

else if (d == STRAIGHT)

{  //for STRAIGHT

analogWrite(Motor\_right\_1, SPEED);

digitalWrite(Motor\_right\_0, LOW);

analogWrite(Motor\_left\_0, SPEED);

digitalWrite(Motor\_left\_1, LOW);

}

#else

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

#endif

else if (d == BACK)

{  //for STRAIGHT

analogWrite(Motor\_right\_0, SPEED);

digitalWrite(Motor\_right\_1, LOW);

analogWrite(Motor\_left\_1, SPEED);

digitalWrite(Motor\_left\_0, LOW);

}

else

{  //for STOP

digitalWrite(Motor\_right\_1, LOW);

digitalWrite(Motor\_right\_0, LOW);

digitalWrite(Motor\_left\_0, LOW);

digitalWrite(Motor\_left\_1, LOW);

}

}

int check\_direction(void)

{

int left,center,right;

left= digitalRead(LEFT\_P);

center= digitalRead(CENTER\_P);

right= digitalRead(RIGHT\_P);

if(left == 1 && center == 0 && right == 1)

return STRAIGHT;

else if(left == 0 && center == 1 && right == 1)

return RIGHT;

else if(left == 1 && center == 1 && right == 0)

return LEFT;

else

return STOP;

}

void loop()

{

int direct = 0;

#if 0

Serial.print("LEFT ");Serial.print(left);

Serial.print(" CENTER ");Serial.print(center);

Serial.print(" RIGHT ");Serial.print(right);

Serial.println(".");

#endif

direct = check\_direction();

Serial.print("direction[left 2][straight 1][right 0]");Serial.print(direct);

if(direct == LEFT)

{

Serial.println("turn right");

turn(RIGHT);

}

else if(direct == STRAIGHT)

{

Serial.println("go straight");

turn(STRAIGHT);

}

else if(direct == RIGHT)

{

Serial.println("turn left");

turn(LEFT);

}

else

{

Serial.println("stop");

turn(STOP);

}

delay(500);

}

----------------------------------------------------------------------------

result

----------------------------------------------------------------------------

direction[left 2][straight 1][right 0][2]turn right

direction[left 2][straight 1][right 0][2]turn right

direction[left 2][straight 1][right 0][2]turn right

direction[left 2][straight 1][right 0][0]turn left

direction[left 2][straight 1][right 0][0]turn left

direction[left 2][straight 1][right 0][1]go straight

----------------------------------------------------------------------------