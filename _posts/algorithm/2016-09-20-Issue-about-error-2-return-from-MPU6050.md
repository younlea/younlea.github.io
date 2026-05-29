---
title: "Issue about error 2 return from MPU6050."
excerpt_separator: "<!--more-->"
date: 2016-09-20 00:25:34 +0900
categories:
  - algorithm
tags:
  - algorithm
  - 알고리즘

toc : true
toc_sticky : true
---

Issue

------------------------------------------

MPU-6050

Read accel, temp and gyro, error = 2

accel x,y,z: -248, 3842, 0

temperature: 113.306 degrees Celsius

gyro x,y,z : 25344, -17385, -16574,

------------------------------------------

MPU6050의 AD0핀을 Arduino의 GND에 연결하면 해결됩니다.

[solution link](http://arduino.stackexchange.com/questions/19473/how-to-solve-error-2-from-the-arduino-mpu6050-accelerometer)

Some MPU6050 device should be connect AD0 to GND(arduino) like below.

![image](/assets/images/posts/6046901/c0028014_57e003424075c.jpg)

MPU6050\_read.... (아래 빨간부분에서 에러를 리턴하는것 같네요)