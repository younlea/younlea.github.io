---
title: "Arduino nano + MPU6050 (arduino.cc 자료)"
excerpt_separator: "<!--more-->"
date: 2016-03-04 01:28:09 +0900
categories:
  - develop_env
tags:
  - develop_env
  - embedded
  - 임베디드

toc : true
toc_sticky : true
---

arduino.cc 자료 입니다.

[관련 링크](http://playground.arduino.cc/Main/MPU-6050#short)

source code

~~`arduino\_nano\_mpu6050.zip`~~ *(이글루스 파일 첨부, 서버 종료로 접근 불가)*

build error 관련 수정 버젼.

arduino 최신으로 가면서 아래 부분이 선언되지 않아서 에러가 나게 되네요. 미리 선언해서 해결하시면 될듯 합니다.

```
int MPU6050_read(int start, uint8_t *buffer, int size);
int MPU6050_write(int start, const uint8_t *pData, int size);
int MPU6050_write_reg(int reg, uint8_t data);
 
 
void setup()
{      
```

수정 사항.~~`arduino\_nano\_mpu6050\_v2.zip`~~ *(이글루스 파일 첨부, 서버 종료로 접근 불가)*수tntnj