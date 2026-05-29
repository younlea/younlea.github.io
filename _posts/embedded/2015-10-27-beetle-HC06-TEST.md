---
title: "beetle HC-06 TEST"
excerpt_separator: "<!--more-->"
date: 2015-10-27 00:57:47 +0900
categories:
  - embedded
tags:
  - dataScience
  - datascience
  - 데이터

toc : true
toc_sticky : true
---

beetle board에 hc-06을 연결해서 들어오는 데이터를 USB port로 보는 코드이다.

```
 
void setup() {
  // initialize serial communication at 9600 bits per second:
  Serial.begin(9600);   //uart
  Serial1.begin(9600);  // bt
  Serial1.println("AT+NAMEbeetle");
}
 
// the loop routine runs over and over again forever:
void loop() {
  if(Serial1.available())
   Serial.println((char)Serial1.read());
}
 
 
```