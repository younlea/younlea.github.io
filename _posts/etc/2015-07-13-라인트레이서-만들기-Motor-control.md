---
title: "라인트레이서 만들기 - Motor control"
excerpt_separator: "<!--more-->"
date: 2015-07-13 23:36:27 +0900
categories:
  - etc
tags:
  - robot
  - 로봇

toc : true
toc_sticky : true
---

![image](/assets/images/posts/5884548/c0028014_55a3ccb48cfa0.png)

output A 와  output B에 각각 모터를 연결한다.

기본 방향 튜닝 테스트 소스 코드..

-----------------------------------------------------------------------------------------

enum direction{

RIGHT = 0,

STRAIGHT,

LEFT

};

void setup()

{

pinMode(2, OUTPUT);

pinMode(3, OUTPUT);

pinMode(4, OUTPUT);

pinMode(5, OUTPUT);

Serial.begin(9600);

}

void turn(int d)

{

if(d == RIGHT)

{

digitalWrite(2, HIGH);

digitalWrite(3, LOW);

digitalWrite(4, HIGH);

digitalWrite(5, LOW);

}

else if ( d == LEFT)

{

digitalWrite(3, HIGH);

digitalWrite(2, LOW);

digitalWrite(5, HIGH);

digitalWrite(4, LOW);

}

else

{  //for STRAIGHT

digitalWrite(3, HIGH);

digitalWrite(2, LOW);

digitalWrite(4, HIGH);

digitalWrite(5, LOW);

}

}

void loop()

{

turn(STRAIGHT);

delay(1000);

}

-----------------------------------------------------------------------------------------