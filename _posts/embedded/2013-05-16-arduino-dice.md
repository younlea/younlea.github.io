---
title: "arduino dice"
excerpt_separator: "<!--more-->"
date: 2013-05-16 02:03:03 +0900
categories:
  - embedded
tags:
  - develop_env
  - embedded
  - 임베디드

toc : true
toc_sticky : true
---

버튼 push시 주사위 값을 보여준다.

button, LED 를 알고 있고 random 함수를 알면 사용 가능

회로도

![image](/assets/images/posts/5744284/c0028014_5193c24809f2e.jpg)

동영상

source

arduino random 함수

<http://arduino.cc/en/Reference/Random>

> #include <Time.h>\
> \
> void setup()\
> {\
>   Serial.begin(9600);\
>   pinMode(2, INPUT);\
>   pinMode(3, OUTPUT);\
>   pinMode(4, OUTPUT);\
>   pinMode(5, OUTPUT);\
>   pinMode(6, OUTPUT);\
>   pinMode(7, OUTPUT);\
>   pinMode(8, OUTPUT);\
>   pinMode(9, OUTPUT);\
> }\
> /\*\
>       9          5 \
>       8     7    4\
>       6          3  \
> \*/\
> \
> /\*   \
>      1 -> 7\
>      2 -> 5,6\
>      3 -> 5,6,7\
>      4 -> 3,5,6,9\
>      5 -> 3,5,6,7,9\
>      6 -> 3,4,5,6,8,9\
> \*/\
> void dice(int num)\
> {\
>   Serial.print(num);\
>   \
>   if(num == 1)\
>   {\
>     digitalWrite(7, 1);\
>   }else if(num == 2)\
>   {\
>       digitalWrite(5, 1);\
>       digitalWrite(6, 1);\
>   }else if(num == 3)\
>   {\
>       digitalWrite(5, 1);\
>       digitalWrite(6, 1);\
>       digitalWrite(7, 1);\
>   }else if(num == 4)\
>   {\
>       digitalWrite(3, 1);\
>       digitalWrite(5, 1);\
>       digitalWrite(6, 1);\
>       digitalWrite(9, 1);\
>   }else if(num == 5)\
>   {\
>       digitalWrite(3, 1);\
>       digitalWrite(5, 1);\
>       digitalWrite(6, 1);\
>       digitalWrite(7, 1);\
>       digitalWrite(9, 1);\
>   }else if (num == 6)\
>   {\
>       digitalWrite(3, 1);\
>       digitalWrite(4, 1);\
>       digitalWrite(5, 1);\
>       digitalWrite(6, 1);\
>       digitalWrite(8, 1);\
>       digitalWrite(9, 1);   \
>   }else\
>   {\
>       digitalWrite(3, 0);\
>       digitalWrite(4, 0);\
>       digitalWrite(5, 0);\
>       digitalWrite(6, 0);\
>       digitalWrite(7, 0);\
>       digitalWrite(8, 0);\
>       digitalWrite(9, 0);  \
>   }\
> }\
> \
> int nu = 0;\
> int push = 0;\
> int toggle = 0;\
> void loop(){\
>      push = digitalRead(2);\
>      \
>      if (push == 1 && toggle == 0)\
>      {\
>        nu = random(1, 7);\
>        dice(nu);\
>        toggle = 1;\
>       }\
>       else if(push == 0 && toggle == 1)\
>       {\
>         dice(10);\
>         toggle = 0;\
>       }\
> }

\