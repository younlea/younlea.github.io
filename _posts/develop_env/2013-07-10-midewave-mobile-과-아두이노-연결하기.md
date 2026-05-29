---
title: "midewave mobile 과 아두이노 연결하기."
excerpt_separator: "<!--more-->"
date: 2013-07-10 00:20:58 +0900
categories:
  - develop_env
tags:
  - develop_env
  - embedded
  - 임베디드

toc : true
toc_sticky : true
---

BT 모듈 셋팅 방법.

BT 모듈 : 실제 싸이트에서 가이드한 버젼 ([BlueSMiRF](http://developer.neurosky.com/docs/doku.php?id=bluesmirf "bluesmirf")) <https://www.sparkfun.com/products/10269>

구매한 버젼  (bluethooth Mate silver retail) <https://www.sparkfun.com/tutorials/264>

BT가 되는 PC 에서 SPP로 연결

Tera term을 받아서 포트 연결. (local echo setting)

$$$ 입력

CMD 리턴 값받고

D                               << 현재 상태를 확인

SP,0000                      << 연결 PW를 0000으로 셋팅

SM,3                          << auto connect master mode

SR,9CB70D90ECFA       <<Midewave mobile mac address  (PC로 연결한후 Mac address를 읽어온다.)

SU,57.6                       <<  baudrate 57600 로 셋팅

D

---                              << save

참고 자료.

https://www.sparkfun.com/datasheets/Wireless/Bluetooth/rn-bluetooth-um.pdf

http://developer.neurosky.com/docs/doku.php?id=mindwave\_mobile\_and\_arduino

자 그럼 연결해 볼까요~~~

헉 잘안되.. 트러블 슈팅.. ㅡ.ㅡ; PC에서 한번 셋팅하면 PC에서 다시 셋팅 불가.. ㅡㅜ

ARDUINO UNO로 연결해서 테스트..

------------------------------------------

<https://www.sparkfun.com/tutorials/264>

------------------------------------------

BT                   Arduino UNO

RTS-0

RX-1        -------    D3

TX-0        -------    D2

VCC        -------    5V

CTS-1

GND         -----     GND

아래처럼 바이너리 만들어서 넣고 연결후 처음 명령 내렸듯이 $$$ 넣으면 CMD 모드로 동작을 한다.

> #include <SoftwareSerial.h>  \
> int bluetoothTx = 2;  // TX-O pin of bluetooth mate, Arduino D2\
> int bluetoothRx = 3;  // RX-I pin of bluetooth mate, Arduino D3\
> SoftwareSerial bluetooth(bluetoothTx, bluetoothRx);\
> \
> void setup()\
> {\
>   Serial.begin(9600);  // Begin the serial monitor at 9600bps\
>   \
> //  bluetooth.begin(115200);  // The Bluetooth Mate defaults to 115200bps\
>   bluetooth.begin(57600);  // 본인이 셋팅한 속도로 셋팅해야 함.\
>   bluetooth.print("$$$");  // Enter command mode\
>   delay(100);  // Short delay, wait for the Mate to send back CMD\
>   bluetooth.println("U,9600,N");  // Temporarily Change the baudrate to 9600, no parity\
>   // 115200 can be too fast at times for NewSoftSerial to relay the data reliably\
>   bluetooth.begin(9600);  // Start bluetooth serial at 9600\
> }\
> \
> void loop()\
> {\
>   if(bluetooth.available())  // If the bluetooth sent any characters\
>   {\
>     // Send any characters the bluetooth prints to the serial monitor\
>     Serial.print((char)bluetooth.read());  \
>   }\
>   if(Serial.available())  // If stuff was typed in the serial monitor\
>   {\
>     // Send any characters the Serial monitor prints to the bluetooth\
>     bluetooth.print((char)Serial.read());\
>   }\
>   // and loop forever and ever!\
> }

> \

어찌됐던.. 난 확인해 보니.. PinCode가 000 이렇게 세자리만 들어가 있었다 ㅡㅜ

\*\*\*Settings\*\*\*

BTA=0006666034DD

BTName=RN42-34DD

Baudrt=57.6

Parity=None

Mode  =Auto

Authen=0

Encryp=0

PinCod=0000

Bonded=0

Rem=9CB70D90ECFA

이후.. mindwave mobile 켜고(그냥 켜면된다.) 그럼 요건 파란불 깜빡깜빡

BT module은 빨간색으로 깜빡이다가 그린으로 바뀐다. 이때 mindwave 도 같이 파란색으로 바뀐다..

이제 다시 데이터를 뽑아 보자.

움.. 아두이노 Uno로 연결해서 software serial을 사용했으나.. 데이터 오는도중에 동작이 안되는 이슈가 생긴다.

다시  Arduino 2560으로 바꾸고.. SMIF에서 오는 신호를 rx1, tx1에 연결해서 아래와 같이 코딩.

자.. 이제 BT 연결되고 mindwave mobile에서 신호가 잘나온다..

나오는 데이터

> PoorQuality: 0 Attention: 44 Time since last packet: 1002\
>                                                          level 4\
> PoorQuality: 0 Attention: 54 Time since last packet: 989\
>                                                         level 5\
> PoorQuality: 0 Attention: 63 Time since last packet: 998\
>                                                         level 6\
> PoorQuality: 0 Attention: 51 Time since last packet: 994\
>                                                         level 5\
> PoorQuality: 0 Attention: 54 Time since last packet: 994\
>                                                         level 5

source code

> ////////////////////////////////////////////////////////////////////////\
> // Arduino Bluetooth Interface with Mindwave\
> // \
> // This is example code provided by NeuroSky, Inc. and is provided\
> // license free.\
> ////////////////////////////////////////////////////////////////////////\
> \
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
>             break;\
>           case 2:    \
>     Serial.println("level 2");\
>             break;\
>           case 3:              \
>     Serial.println("level 3");\
>             break;\
>           case 4:\
>     Serial.println("level 4");\
>             break;\
>           case 5:\
>     Serial.println("level 5");\
>             break;\
>           case 6:              \
>     Serial.println("level 6");\
>             break;\
>           case 7:\
>     Serial.println("level 7");\
>             break;\
>           case 8:\
>     Serial.println("level 8");\
>             break;\
>           case 9:\
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

자.. 내일은 여기서 필요한 데이터만 뽑아 보자구..