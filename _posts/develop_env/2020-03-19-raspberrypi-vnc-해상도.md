---
title: "raspberrypi vnc 해상도"
excerpt_separator: "<!--more-->"
date: 2020-03-19 17:36:28 +0900
categories:
  - develop_env
tags:
  - develop_env
  - embedded
  - 임베디드

toc : true
toc_sticky : true
---

VNC로 접속할때 해상도가 작아서 좀 불변한데 해상도를 아래 처럼 바꿔서 하면 됩니다. ^^;

sudo vim /boot/config.txt

![image](/assets/images/posts/6620883/c0028014_5e732ed4d4ebb.png)

sudo reboot

\