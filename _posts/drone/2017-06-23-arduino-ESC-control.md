---
title: "arduino ESC control"
excerpt_separator: "<!--more-->"
date: 2017-06-23 23:02:05 +0900
categories:
  - drone
tags:
  - drone
  - 드론

toc : true
toc_sticky : true
---

<https://dronesandrovs.wordpress.com/2012/11/24/how-to-control-a-brushless-motor-esc-with-arduino/>

가변저항을 이용해 모터 제어 .

가변 저항 값을 A0에서 받고 모터 제어 신호는 D9에서 보내서 컨트롤 하는것으로 구현 됨.

![image](/assets/images/posts/6144861/c0028014_594d21f0eb81a.jpg)

![image](/assets/images/posts/6144861/c0028014_594d220e6442c.jpg)

```
#include <Servo.h>
 
Servo esc;
int throttlePin = 0;
 
void setup()
{
esc.attach(9);
Serial.begin(9600);
}
 
void loop()
{
int throttle = analogRead(throttlePin);
 
throttle = map(throttle, 0, 1023, 0, 179);
Serial.println(throttle);
esc.write(throttle);
}
```