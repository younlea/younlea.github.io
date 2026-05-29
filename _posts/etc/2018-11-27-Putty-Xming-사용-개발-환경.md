---
title: "Putty + Xming 사용 개발 환경"
excerpt_separator: "<!--more-->"
date: 2018-11-27 14:16:47 +0900
categories:
  - etc
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

mobaXterm을 집에서 쓰기는 하는데 회사에서 쓰기에는 라이센스 이슈가 있어서 그냥 putty + Xming을 깔아서 windows PC 에서

사용합니다.

terminal 을 terminator를 사용하면 좋네요 ^^;

Putty and Xming setting guide

<http://www.geo.mtu.edu/geoschem/docs/putty_install.html>

Connection -> SSH -> X11 -> enable X11 forwarding.

Xdisplay location : localhost:0.0

[putty download 하는 곳](https://www.putty.org/)

[Xming donwload 하는 곳](https://sourceforge.net/projects/xming/)

\