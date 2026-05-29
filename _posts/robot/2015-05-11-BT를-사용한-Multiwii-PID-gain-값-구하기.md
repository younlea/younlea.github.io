---
title: "BT를 사용한 Multiwii PID gain  값 구하기"
excerpt_separator: "<!--more-->"
date: 2015-05-11 23:25:03 +0900
categories:
  - robot
tags:
  - robot
  - 로봇

toc : true
toc_sticky : true
---

야외에서 PC를 가져가지 않고 BT로 테스트 하기 위한 방법이다.

[BT 연결하기](https://younlea.github.io/Embedded/BT-setting-가이드/)

연결시 HC-06 접속 속도를 115200으로 바꿔서 연결해야지만 정상적으로 연결이 된다. [셋팅방법](https://www.squirrel-labs.net/blog/hc-06-bluetooth-module-changing-baudrate-etc/)

BT가 되면 EZ-GUI android tool을 이용해서 PID gain값을 수정하여 진행 가능하다.

^^ PC랑 연결하지 않아도 되니.. 참 좋다 ^^;

serial protocol.

<http://www.multiwii.com/wiki/index.php?title=Multiwii_Serial_Protocol>

\