---
title: "arduino servo motor 제어"
excerpt_separator: "<!--more-->"
date: 2013-05-29 00:40:04 +0900
categories:
  - embedded
tags:
  - robot
  - 로봇

toc : true
toc_sticky : true
---

목적 : servo motor를 제어 해본다.

회로 구성

![image](/assets/images/posts/5747100/c0028014_51a4cfc7502c1.jpg)

소스 코드  : 0 ~ 180 를 반복하는 코드를 짜본다.

> #include <Servo.h>\
> \
> Servo myServo;\
> \
> int potVal;\
> int angle = 0;\
> \
> void setup(){\
>   myServo.attach(9);\
>   Serial.begin(9600);\
> }\
> \
> \
> void loop()\
> {\
>  if(angle < 180 )\
>  {\
>     myServo.write(angle);\
>     delay(15);\
>     angle = angle+1;\
>  }else\
>  {\
>     angle = 0;\
>  }\
>   Serial.print(", angle: ");\
>   Serial.println(angle);\
>   \
> }

동영상

\