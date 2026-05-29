---
title: "FIRL LEPTON V3 porting guide"
excerpt_separator: "<!--more-->"
date: 2016-10-04 23:02:02 +0900
categories:
  - develop_env
tags:
  - develop_env
  - embedded
  - 임베디드

toc : true
toc_sticky : true
---

lepton sensor (V2) porting guide on raspberrypi

<https://groupgets.com/blog/posts/8-installation-guide-for-pure-breakout-board-on-raspberry-pi-2>   << suggested guide

<http://www.appropedia.org/How_to_install_FLIR_Lepton_Thermal_Camera_and_applications_on_Raspberry_Pi>

<https://learn.sparkfun.com/tutorials/flir-lepton-hookup-guide>  << SPI CE PIN issue. (you should change CE1(GPIO7) to CE0(GPIO8))

I2C & SPI config

<https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c>

<http://www.raspberrypi-spy.co.uk/2014/11/enabling-the-i2c-interface-on-the-raspberry-pi/>

Raspberrypi application source code.

<https://github.com/groupgets/LeptonModule>

[https://github.com/groupgets/LeptonModule/wiki](https://github.com/groupgets/LeptonModule/wiki )

<http://www.pureengineering.com/projects/lepton>

V3 developer guide

<http://www.pureengineering.com/projects/purethermal1>

What is different between V3 and V2

<http://www.mako.co.kr/--->

lepton SDK

<https://groupgets.com/manufacturers/flir/products/flir-lepton>   << for V2

~~`Lepton\_Intro.zip`~~ *(이글루스 파일 첨부, 서버 종료로 접근 불가)*  << v2 sdk

lepton google group

<https://groups.google.com/forum/#!forum/flir-lepton>

Currently, FLIR didnot release V3 data sheet.

I will ask to this link.

<http://flir-kr.custhelp.com/app/utils/login_form/redirect/ask>

<http://www.flir.com/cores/display/?id=53135>

lepton v3 data sheet

~~`Lepton3\_datasheet.pdf`~~ *(이글루스 파일 첨부, 서버 종료로 접근 불가)*