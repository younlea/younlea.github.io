---
title: "쿼드콥터 source code (Nanowii 사용시)"
excerpt_separator: "<!--more-->"
date: 2015-08-16 23:34:21 +0900
categories:
  - drone
tags:
  - drone
  - 드론

toc : true
toc_sticky : true
---

아래 define을 추가 하면..

![image](/assets/images/posts/5888191/c0028014_55d09f13a500b.png)

def.h에서 아래 부분이 동작해서 축에 대한 셋팅은 완료 된다. ^^;

![image](/assets/images/posts/5888191/c0028014_55d09f4b19652.png)

config 수정

PPM신호 종류 선택

// #define SERIAL\_SUM\_PPM         ROLL,PITCH,THROTTLE,YAW,AUX1,AUX2,AUX3,AUX4,8,9,10,11 //For Robe/Hitec/Futaba

#define MPU6050\_LPF\_42HZ

**arduino target을  leonardo로 꼭 바꾸고 다운로드 해야함. 안그럼 보드 뻗음 ㅜㅜ**

**\**

**ESC calibration 동영상 (****simonk 펌웨어 emax esc)**

**https://youtu.be/E6epG5E8gAQ**

**\**

**board 살리기(nanowii SPI 통신 port)**

**http://gogoprivateryan.blogspot.kr/2014/08/nanowii-arduino-nanowii-mpu6050.html**

**\**

**\**

**turnigy original version connection method------------------------------**

**turnigy bind**

**<https://youtu.be/z_-ruNd1FcU>**

**\**

**receiver pwm setting**

**https://www.multicopters.co.uk/tutorials/multiwii-beginners-guide-part-2-transmitter-receiver**

**\**

**receiver 종류**

**http://blog.oscarliang.net/pwm-ppm-sbus-dsm2-dsmx-sumd-difference/**

**\**

**pwm 설정시 아래 주석해줘야 함.**

**//#define SPEKTRUM 1024**

**------------------------------------------------------------------------------------**

**\**

**main board 는 상판에 달면 안되고 아래 damper가 좀더 있어야 할것으로 보임.**

**\**

**최종 코드**

~~`working\_nanowii.zip`~~ *(이글루스 파일 첨부, 서버 종료로 접근 불가)*

**\**