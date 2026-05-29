---
title: "arduino UNO + HC-06"
excerpt_separator: "<!--more-->"
date: 2014-07-01 00:20:32 +0900
categories:
  - embedded
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

Setting test code

Uno have one serial port (TX0, RX0)

But I want to test both BT and USB serial port..

PC ---> arduino --> BT

^

|  I want use USB on this area.

So I use softwareSerial code, This code is provide from arduino library.

This is sample code

|  |  |
| --- | --- |
| 1  2  3  4  5  6  7  8  9  10  11  12  13  14  15  16  17  18  19  20  21  22  23  24  25  26  27  28  29  30  31 | #include <SoftwareSerial.h>  int bluetoothTx = 2;  // TX-O pin of bluetooth mate, Arduino D2  int bluetoothRx = 3;  // RX-I pin of bluetooth mate, Arduino D3  SoftwareSerial bluetooth(bluetoothTx, bluetoothRx);    void setup()  {  Serial.begin(9600);  // Begin the serial monitor at 9600bps    //  bluetooth.begin(115200);  // The Bluetooth Mate defaults to 115200bps  //   bluetooth.print("$$$");  // Enter command mode  //  delay(100);  // Short delay, wait for the Mate to send back CMD  //  bluetooth.println("U,9600,N");  // Temporarily Change the baudrate to 9600, no parity  // 115200 can be too fast at times for NewSoftSerial to relay the data reliably  bluetooth.begin(9600);  // Start bluetooth serial at 9600  }    void loop()  {  if(bluetooth.available())  // If the bluetooth sent any characters  {  // Send any characters the bluetooth prints to the serial monitor  Serial.print((char)bluetooth.read());  }  if(Serial.available())  // If stuff was typed in the serial monitor  {  // Send any characters the Serial monitor prints to the bluetooth  bluetooth.print((char)Serial.read());  }  // and loop forever and ever!  } |

And I need connect HW

HC-06             arduino

TX   -----------  PIN2

RX   -----------  PIN3

VDD -----------  3.3V

GND ----------- GND