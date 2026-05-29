---
title: "Multiwill source를 이용해서 작업 시작"
excerpt_separator: "<!--more-->"
date: 2015-01-21 00:32:51 +0900
categories:
  - embedded
tags:
  - drone
  - 드론

toc : true
toc_sticky : true
---

자.. 지지부진 하던 작업을 다시 시작해 보려고 한다.

일단 hobbyking에서 산 부품( boddy frame) + arduino + mpu6050 + 조종기&송신기(Turnigy 9X) + 수신기(D4Fr FrSky)

![image](/assets/images/posts/5863149/c0028014_54be77a8c79d3.png)

대충 위와 같이 구조가 될텐데.. 일단 SW는 Multiwii를 사용해서 작업 해보려고 한다.

기본 작업후 추후 진행 예정. (일단 띄우고 조종하는게 되면 그 다음 단계로 갈수 있을듯 하니까..)

RF 모듈의 경우 지금은 RC 조종기를 사용하지만 추후 WIFI나 BT를 사용도 해볼 예정이다. (flexbot 을 참고해서 ^^)

Main CPU의 경우도 arduino에서 라즈베리 파이나 비글본 블랙으로 바꿔볼 예정이다.

Multiwill

<http://www.multiwii.com/>

source code

<https://code.google.com/p/multiwii/>

google code가 막힌 분들을 위해 최신 stable code (Multiwii 2.3)을 공유합니다.

한번에 10MB를 못올려서 arduino에 들어가는 코드만 올립니다. processing 을 사용한 테스트 툴은 google code에서 받으시길..

~~`MultiWii\_arduino\_code.7z`~~ *(이글루스 파일 첨부, 서버 종료로 접근 불가)*

step 1. Multiwii를 아두이노에 올리고 RF 연결을 테스트 한다.

step 2. MPU6050에서 센서 데이터를 받고 필터 처리가 제대로 되는지 확인한다.

step 3. HW 조립후 아두이노와 ESC- motor를 연결하는 작업을 하고 Motor 동작 확인한다.

step 4. gain 값 보정후 날려본다.

위와 같이 큰 단위로 나눠서 진행해 볼 예정이다.

일단 step1부터... 움.. PPM이라는데... 용어 정리도 하면서 해야 것다 ^^;

\