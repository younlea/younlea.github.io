---
title: "avrdude write issue"
excerpt_separator: "<!--more-->"
date: 2016-01-05 20:05:54 +0900
categories:
  - robot
tags:
  - robot
  - 로봇

toc : true
toc_sticky : true
---

avrdude로 write 할 때 종종

WinAVR-20100110 검색후 [다운로드](http://winavr.sourceforge.net/download.html)

avrdude -c usbtiny -p atmega328p -b 115200 -B 4 -e -u 이후 아래 에러가 날 경우

issue : Expected signature for ATMEGA328P is 1E 95 0F

avr에서 device 값을 못 읽어오거나 틀린값을 읽어올 경우의 에러 입니다.

<http://www.instructables.com/id/Bootload-an-ATmega328/?ALLSTEPS>  << 해결책

위 해결책으로 해도 아래 에러가 날 경우..

issue : avrdude: Device signature = 0x000000

<http://forum.arduino.cc/index.php?topic=25385.0>  << 해결책

<http://gorillarobotics.blogspot.kr/2008/12/fixing-device-signature-0x000000.html>

만약 이렇게 해도 안될 경우 움.. 오실레이터가 망가진것 같은데.. 오실레이터에서 신호가 나오는지 찍어 봐야 합니다.

\