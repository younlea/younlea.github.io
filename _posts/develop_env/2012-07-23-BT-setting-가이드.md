---
title: "BT setting 가이드.."
excerpt_separator: "<!--more-->"
date: 2012-07-23 12:54:40 +0900
categories:
  - develop_env
tags:
  - develop_env
  - embedded
  - 임베디드

toc : true
toc_sticky : true
---

BT module

Atmega128의 UART1번에 TX RX를 물리고.. VDD와 GND만 연결할 예정이다.

잘되야 할텐데.. 하하. 회로 구성은.. 이것참 ORCAD 깔아야 하나 ㅜㅜ

![image](/assets/images/posts/5660394/c0028014_500eab263187c.jpg)

**Atmega128 TX1, RX1을 다이렉트로 연결할예정이고 VDD는 3.0V를 사용한다.**

[ATmega128 ] ------------------------[블루투스모듈]

VCC  -------------------------------------3.3V

RXD1(27) <------------------------------- TXD\
TXD1(28) -------------------------------->RXD

GND ------------------------------------ GND

움. TX RX를 다이렉트로 물려도 문제는 없을듯한데.. 움움.. 내일 고민하고 바로 회로 결선 들어가 보자.

자료 : <http://cafe.naver.com/mpucafe/2596>

PW : 1234

Android 예제 어플 setting guide... 아래 값으로 셋팅해야지만 Serial 통신이 된단다.

File: BluetoothUuid.java (frameworks\base\core\java\android\bluetooth)\

       AudioSink = ("00001105-0000-1000-8000-00805f9b34fb")\

       FileTransfer = ("00001106-0000-1000-8000-00805f9b34fb")\
       PhoneBookAccess = ("00001130-0000-1000-8000-00805f9b34fb")\
       BasicPrinting = ("00001122-0000-1000-8000-00805f9b34fb")\
       **SerialPort = ("00001101-0000-1000-8000-00805f9b34fb")\**       DUN = ("00001103-0000-1000-8000-00805f9b34fb")\
       SIM\_ACC = ("0000112D-0000-1000-8000-00805F9B34FB")\
       GenericAudio = ("00001203-0000-1000-8000-00805F9B34FB")\
       HSPAG = ("00001112-0000-1000-8000-00805F9B34FB")\
       HandsfreeAG = ("0000111F-0000-1000-8000-00805F9B34FB")\
       HID = ("00001124-0000-1000-8000-00805f9b34fb")\
       PANU = ("00001115-0000-1000-8000-00805f9b34fb")\
       NAP= ("00001116-0000-1000-8000-00805f9b34fb")\
       GN = ("00001117-0000-1000-8000-00805f9b34fb")\
       SYNC\_ = ("00001104-0000-1000-8000-00805F9B34FB")\
       CTP= ("00001109-0000-1000-8000-00805F9B34FB")\
       ICP = ("00001110-0000-1000-8000-00805F9B34FB")\
       FAX = ("00001111-0000-1000-8000-00805F9B34FB")\
       LAP = ("00001102-0000-1000-8000-00805F9B34FB")\
       BIP = ("0000111A-0000-1000-8000-00805F9B34FB")\
       VIDEO\_DIST = ("00001305-0000-1000-8000-00805F9B34FB")

수정은.

BluetoothChatService.java에서 아래처럼...

    // Unique UUID for this application

    //private static final UUID MY\_UUID = UUID.fromString("fa87c0d0-afac-11de-8a39-0800200c9a66");

    private static final UUID MY\_UUID = UUID.fromString("00001101-0000-1000-8000-00805f9b34fb");

흠... SDK는 API10을 써야 한다. 이전꺼라..