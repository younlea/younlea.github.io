---
title: "뇌파 - 아두이노 - flying ball"
excerpt_separator: "<!--more-->"
date: 2013-07-11 01:01:13 +0900
categories:
  - robot
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

뇌파 -> 아두이노 -> flying ball

IR을 뚫으려고 했으나 잘안되서 급하게 모터로 제어기를 제어하도록 코딩함..

추후 IR을 뚫으면 행복할것 같다.

뇌파의 세기도 각양각색이라. 고민을 해야할것 같다.

우선 테스트코드

> \
> #include <Servo.h>\
> Servo myServo;\
> \
> \
> #define BAUDRATE 57600\
> \
> \
> // checksum variables\
> byte generatedChecksum = 0;\
> byte checksum = 0; \
> int payloadLength = 0;\
> byte payloadData[64] = {0};\
> byte poorQuality = 0;\
> byte attention = 0;\
> byte meditation = 0;\
> \
> // system variables\
> long lastReceivedPacket = 0;\
> boolean bigPacket = false;\
> \
> //////////////////////////\
> // Microprocessor Setup //\
> //////////////////////////\
> void setup() {\
>   myServo.attach(9);\
>   myServo.write(10);\
> \
>   Serial.begin(9600);\
>   Serial1.begin(BAUDRATE);           // midewave mobile\
> }\
> \
> /////////////////////////////////\
> // Read data from Serial1 UART //\
> /////////////////////////////////\
> byte ReadOneByte() {\
>   int ByteRead;\
>     \
>   while(!Serial1.available());\
>     ByteRead = Serial1.read();\
> //for debug\
> //  Serial.print((char)ByteRead);   // echo the same byte out the USB serial (for debug purposes)\
> \
>   return ByteRead;\
> }\
> \
> /////////////\
> //MAIN LOOP//\
> /////////////\
> void loop() {\
> \
> \
>   // Look for sync bytes\
>   if(ReadOneByte() == 170) {\
>     if(ReadOneByte() == 170) {\
>       payloadLength = ReadOneByte();\
>       if(payloadLength > 169)                      //Payload length can not be greater than 169\
>           return;\
> \
>       generatedChecksum = 0;        \
>       for(int i = 0; i < payloadLength; i++) {  \
>         payloadData[i] = ReadOneByte();            //Read payload into memory\
>         generatedChecksum += payloadData[i];\
>       }   \
> \
>       checksum = ReadOneByte();                      //Read checksum byte from stream      \
>       generatedChecksum = 255 - generatedChecksum;   //Take one's compliment of generated checksum\
> \
>         if(checksum == generatedChecksum) {    \
> \
>         poorQuality = 200;\
>         attention = 0;\
>         meditation = 0;\
> \
>         for(int i = 0; i < payloadLength; i++) {    // Parse the payload\
>           switch (payloadData[i]) {\
>           case 2:\
>             i++;            \
>             poorQuality = payloadData[i];\
>             bigPacket = true;            \
>             break;\
>           case 4:\
>             i++;\
>             attention = payloadData[i];                        \
>             break;\
>           case 5:\
>             i++;\
>             meditation = payloadData[i];\
>             break;\
>           case 0x80:\
>             i = i + 3;\
>             break;\
>           case 0x83:\
>             i = i + 25;      \
>             break;\
>           default:\
>             break;\
>           } // switch\
>         } // for loop\
> \
>         // \*\*\* Add your code here \*\*\*\
> \
>         if(bigPacket) {\
>           Serial.print("PoorQuality: ");\
>           Serial.print(poorQuality, DEC);\
>           Serial.print(" Attention: ");\
>           Serial.print(attention, DEC);\
>           Serial.print(" Time since last packet: ");\
>           Serial.print(millis() - lastReceivedPacket, DEC);\
>           lastReceivedPacket = millis();\
>           Serial.print("\n");\
>           \
>            switch(attention / 10) {\
>           case 0:\
>     Serial.println("level 0");\
>             break;\
>           case 1:\
>     Serial.println("level 1");\
>     myServo.write(10);\
>             break;\
>           case 2:    \
>     Serial.println("level 2");\
>     myServo.write(20);\
>             break;\
>           case 3:              \
>     myServo.write(30);\
>     Serial.println("level 3");\
>             break;\
>           case 4:\
>     myServo.write(40);\
>     Serial.println("level 4");\
>             break;\
>           case 5:\
>     myServo.write(50);\
>     Serial.println("level 5");\
>             break;\
>           case 6:          \
>     myServo.write(60);    \
>     Serial.println("level 6");\
>             break;\
>           case 7:\
>     myServo.write(70);\
>     Serial.println("level 7");\
>             break;\
>           case 8:\
>     myServo.write(80);\
>     Serial.println("level 8");\
>             break;\
>           case 9:\
> //    myServo.write(90);\
>     Serial.println("level 9");\
>             break;\
>           case 10:\
>     Serial.println("level 10");\
>             break;\
>           }  \
>         }\
>        \
>         bigPacket = false;        \
>       }\
>       else {\
>         // Checksum Error\
>       }  // end if else for checksum\
>     } // end if read 0xAA byte\
>   } // end if read 0xAA byte\
> }\
> \
> \
> > \

\