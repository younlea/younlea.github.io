---
title: "20130430_School_bell_piezo_LED_serial"
excerpt_separator: "<!--more-->"
date: 2013-04-30 01:36:03 +0900
categories:
  - embedded
tags:
  - robot
  - 로봇

toc : true
toc_sticky : true
---

어제에 이어 업그레이드..

serial로 신호가 오면 학교종종 땡땡땡을 연주하도록 하였고

각 음계에 맞춰서 LED도 나오도록 구현하였음.

회로도

![image](/assets/images/posts/5740526/c0028014_517ea0fa41935.jpg)

Source code

> #define DO3 7645\
> #define RE3 6811\
> #define MI3 6068\
> #define PA3 5727\
> #define SOL3 5102\
> #define RA3 4545\
> #define SI3 4050\
> #define DO4 3822\
> #define RE4 3405\
> #define MI4 3034\
> #define PA4 2863\
> #define SOL4 2551\
> #define RA4 2273\
> #define SI4 2025\
> #define DO5 1910\
> #define RE5 1703\
> #define MI5 1517\
> #define PA5 1432\
> #define SOL5 1276\
> #define RA5 1136\
> #define SI5 1012\
> \
> \
> \
> void LED\_Control(int OnOff)\
> {\
>   digitalWrite(2,OnOff);\
>   digitalWrite(3,OnOff);\
>   digitalWrite(4,OnOff);\
>   digitalWrite(5,OnOff);\
>   digitalWrite(6,OnOff);\
>   digitalWrite(7,OnOff);\
>   digitalWrite(8,OnOff);\
> }\
> \
> void LED\_Control\_SCALE(int SCALE)\
> {\
>   if(SCALE == DO4)\
>   {\
>     digitalWrite(2, 1);\
>   }\
>   if(SCALE == RE4)\
>   {\
>       digitalWrite(2,1);\
>       digitalWrite(3,1);      \
>   }\
>   if(SCALE == MI4)\
>   {\
>       digitalWrite(2,1);\
>       digitalWrite(3,1);      \
>       digitalWrite(4,1);      \
>   }  \
>   if(SCALE == PA4)\
>   {\
>       digitalWrite(2,1);\
>       digitalWrite(3,1);      \
>       digitalWrite(4,1);      \
>       digitalWrite(5,1);      \
>   }  \
>   if(SCALE == SOL4)\
>   {\
>       digitalWrite(2,1);\
>       digitalWrite(3,1);      \
>       digitalWrite(4,1);      \
>       digitalWrite(5,1);      \
>       digitalWrite(6,1);      \
>   }  \
>   if(SCALE == RA4)\
>   {\
>       digitalWrite(2,1);\
>       digitalWrite(3,1);      \
>       digitalWrite(4,1);      \
>       digitalWrite(5,1);      \
>       digitalWrite(6,1);      \
>       digitalWrite(7,1);      \
>   }  \
>   if(SCALE == SI4)\
>   {\
>       digitalWrite(2,1);\
>       digitalWrite(3,1);      \
>       digitalWrite(4,1);      \
>       digitalWrite(5,1);      \
>       digitalWrite(6,1);      \
>       digitalWrite(7,1);      \
>       digitalWrite(8,1);      \
>   }  \
> }\
> void play\_music(int SCALE, int duration)\
> {\
>   LED\_Control\_SCALE(SCALE);\
>   for(long i = 0; i<1000000; i=i+SCALE)\
>   {\
>       digitalWrite(13,1);\
>       delayMicroseconds(SCALE/2);\
>       digitalWrite(13,0);\
>       delayMicroseconds(SCALE/2);\
>   }\
>   delay(duration);\
>   LED\_Control(0);\
> }\
> \
> void school\_bell(int tempo)\
> {\
>    play\_music(SOL4, 50\*tempo);\
>    play\_music(SOL4, 50\*tempo);\
>    play\_music(RA4, 50\*tempo);\
>    play\_music(RA4, 50\*tempo);\
>    play\_music(SOL4, 50\*tempo);\
>    play\_music(SOL4, 50\*tempo);\
>    play\_music(MI4, 50\*tempo);\
>    play\_music(SOL4, 50\*tempo);\
>    play\_music(SOL4, 50\*tempo);\
>    play\_music(MI4, 50\*tempo);\
>    play\_music(MI4, 50\*tempo);\
>    play\_music(RE4, 100\*tempo);\
> \
>    play\_music(SOL4, 50\*tempo);\
>    play\_music(SOL4, 50\*tempo);\
>    play\_music(RA4, 50\*tempo);\
>    play\_music(RA4, 50\*tempo);\
>    play\_music(SOL4, 50\*tempo);\
>    play\_music(SOL4, 50\*tempo);\
>    play\_music(MI4, 50\*tempo);\
>    play\_music(SOL4, 50\*tempo);\
>    play\_music(MI4, 50\*tempo);\
>    play\_music(RE4, 50\*tempo);\
>    play\_music(MI4, 50\*tempo);\
>    play\_music(DO4, 200\*tempo);\
>    \
>    delay(10000);\
> }\
> \
> void setup()\
> {\
>   //GPIO 2~8 -> LED control\
>   pinMode(2, OUTPUT);\
>   pinMode(3, OUTPUT);\
>   pinMode(4, OUTPUT);\
>   pinMode(5, OUTPUT);\
>   pinMode(6, OUTPUT);\
>   pinMode(7, OUTPUT);\
>   pinMode(8, OUTPUT);\
>                         \
>   //GPIO 13 -> PIEZO control\
>   pinMode(13, OUTPUT);\
>   \
>   Serial.begin(9600);\
> }\
> \
> \
> void loop(){\
> //  school\_bell(1);  \
>  if(Serial.available())\
>  {\
>    char ch = Serial.read();\
>    if( ch == 'a')\
>    {\
>        Serial.print("School bell start \n");\
>        school\_bell(1);\
>    }\
>    if(ch == 'b')\
>    {\
>        Serial.print("LED Turn Off\n");\
>    }\
>  }\
> \
> }

\