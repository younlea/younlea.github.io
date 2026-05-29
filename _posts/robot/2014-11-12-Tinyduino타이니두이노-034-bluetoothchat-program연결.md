---
title: "Tinyduino(타이니두이노) - 03-4. bluetoothchat program연결"
excerpt_separator: "<!--more-->"
date: 2014-11-12 23:29:52 +0900
categories:
  - robot
tags:
  - robot
  - 로봇

toc : true
toc_sticky : true
---

어제 산뜻하게 bluetooth chat 프로그램도 빌드해서 올리고....

오늘와서 아래처럼 serial로 코드도 바꾸고..\
[https://younlea.github.io/Embedded/BT-setting-가이드/](https://younlea.github.io/Embedded/BT-setting-가이드/)

그런데... device connect가 안된다.. 왜 일까?????

자.. 오늘 연결만 되면 끝나는건데.. 오늘은 성공하길 ^^;

2014/11/13.. 하루 지나서...

대충 어제 왜 삽질을 했는지 알게 되었다... ㅡㅡ;

첫째. BLE에는 SPP가 없다... 이게 제일 큰 충격.. 이전에 쓰던 bluetooth3.0처럼 연결하고 그냥 쓸수 없다는거다., ㅜㅜ

둘째. 어제 Note8.0에서 search는 되는데 연결 안 되었던 이유...

- 기본적으로 BLE는 mouse랑 keyboard같은 HID만 지원한단다. 그래서 연결 안된다는거였고..

-BLE 전용 어플을 사용해서 connect 테스트 가능하다고 함.

셋째. 안드로이드 어플도 BLE와 BT 가 다르단다..

맨땅해딩했구만..

집에와서 안드로이드 어플중 "nRF Master Control" 를 찾아서 테스트해본 결과다.. 연결까지 되고 정보도 읽어온다.

![image](/assets/images/posts/5854317/c0028014_5464c3a78a97a.png)

![image](/assets/images/posts/5854317/c0028014_5464c3b0d342d.png)

connect 하게 되면..

![image](/assets/images/posts/5854317/c0028014_5464c3c130c07.png)

![image](/assets/images/posts/5854317/c0028014_5464c3c69d7f3.png)

![image](/assets/images/posts/5854317/c0028014_5464c3ced1874.png)

![image](/assets/images/posts/5854317/c0028014_5464c3d684fd5.png)

![image](/assets/images/posts/5854317/c0028014_5464c3db43de3.png)

문제는 여기서 SPP와 비슷한 역할을 하는 UART 2.0이 있느냐인건데 ....

https://www.nordicsemi.com/Products/nRFready-Demo-APPS

nRF UART 2.0Bluetooth Smart Application for UART service

아래는 dfrobot에서 bluno 라는 프로그램을 제공한다는데 이걸로 확인해 보자.

http://www.dfrobot.com/wiki/index.php/Bluno\_SKU:DFR0267

http://www.dfrobot.com/index.php?route=product/product&product\_id=1044

\