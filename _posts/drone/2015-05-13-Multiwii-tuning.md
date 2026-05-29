---
title: "Multiwii tuning"
excerpt_separator: "<!--more-->"
date: 2015-05-13 00:06:11 +0900
categories:
  - drone
tags:
  - robot
  - 로봇

toc : true
toc_sticky : true
---

PID 값 – 기울여서 원자세로 돌아오는 반응속도 관련 직접 손으로 잡고 PID gain값 확인 및 보정 하면 됨.

Gyro, acc 둘다 정상적으로 반응을 하는지 동작 확인 필요

Gyro:각 방향으로 돌아갈 때 역 방향으로 힘이 주어지는지

Acc:기울어져 있을 때 기준 값으로 가려고 하는지

최초 가속도 calibration은 수평이 맞는 곳에서 하도록 하고 떴을때 흐르는 것은 조종기 trim으로 roll, pitch, yaw 영점을 맞춘다.