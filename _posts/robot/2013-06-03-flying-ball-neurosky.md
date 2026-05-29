---
title: "flying ball + neurosky"
excerpt_separator: "<!--more-->"
date: 2013-06-03 21:41:37 +0900
categories:
  - robot
tags:
  - robot
  - 로봇

toc : true
toc_sticky : true
---

한번 플라잉 볼을 뇌파로 띄웠다 내렸다 해볼까???

flying ball

[https://younlea.github.io/DataScience/새로운-도전flying-ball-휴대폰으로-제어하기/](https://younlea.github.io/DataScience/새로운-도전flying-ball-휴대폰으로-제어하기/)

neurosky EGG 센서

<http://www.neurosky.com/>

arduino

neurosky -> arduino -> flying ball  이렇게 컨트롤 할 예정이다.

1. neurosky **MindWave Mobile** 구매 및 PC나 아두이노에서 데이터 받아서 처리.

<http://developer.neurosky.com/docs/doku.php?id=mindwave_mobile_and_arduino>

BlueSMiRF : arduin <->neurosky  통신용 chip

<http://roboholic1.godo.co.kr/shop/goods/goods_view.php?goodsno=103&inflow=naver&NaPm=ct%3Dhhhnic3s%7Cci%3Da47fbefc7927e313c97e6a8c7dcd521f5efd1695%7Ctr%3Dslsl%7Csn%3D188145%7Chk%3D033d50bede6d2644f342c00efc1ae6ea1ba359a5>

2. flying ball IR 통신 방법 hacking.

http://grinbee.woobi.co.kr/wiki/doku.php?id=rc\_helicopter\_autopilot\_project

추가로 아래도 구현해 볼 생각이다.

neurosky data를 arduino를 거쳐 processing로 처리해서 눈으로 보이도록 구현.

example

<http://blog.arduino.cc/category/sensors/mindwave/>

<http://developer.neurosky.com/docs/lib/exe/fetch.php?media=mindwave_arduino_leds.pdf>

결국 이걸 만들게 되는구나..

<http://www.mindtecstore.com/index.php/de/puzzlebox-orbit-set>

20130620 구매 목록

brainwave-starter-kit 2개 - http://store.neurosky.com/products/brainwave-starter-kit

BlueSMiRF Silver 2개 - <http://artrobot.co.kr/front/php/product.php?product_no=678&main_cate_no=&display_group=>

적외선 송수신기 5개 - <http://artrobot.co.kr/front/php/product.php?product_no=426&main_cate_no=38&display_group=1>

Arduion board

arduino-duo 1대          - <http://artrobot.co.kr/front/php/product.php?product_no=497&main_cate_no=7&display_group=1>

arduino-mega 2560 1대 - <http://artrobot.co.kr/front/php/product.php?product_no=160&main_cate_no=7&display_group=1>

\