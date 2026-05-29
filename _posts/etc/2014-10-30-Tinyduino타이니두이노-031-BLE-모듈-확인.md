---
title: "Tinyduino(타이니두이노) - 03-1. BLE 모듈 확인"
excerpt_separator: "<!--more-->"
date: 2014-10-30 23:51:25 +0900
categories:
  - etc
tags:
  - etc

toc : true
toc_sticky : true
---

Bluetooth module을 BLE module 로 [메카쏠루션](http://www.mechasolution.com/shop/goods/goods_view.php?goodsno=467&category=001011)에서 구매했다.

![image](/assets/images/posts/5852488/c0028014_54524f1b982a7.png)

그런데 관련 자료가 없다.. ㅡ.ㅡ; 업체에 전화를 해도.. 오후 두시까지만 통화가 된단다 ㅡㅜ

결국 부품을 찾다.. 지난번에도 얘기 했지만 Kickstarter에 올리고 만든 회사가 ... 그러니까 본사 싸이트가 있더라..\
거기에 이 보드에 대한 정보가 딱~~~ 하고 있다..\
https://tiny-circuits.com/tiny-shield-bluetooth-low-energy.html\
![image](/assets/images/posts/5852488/c0028014_5452500edca85.png)

자.. 그런데.. 예제 코드는 어디 있는것인가??? 통신할 폰에서 테스트용 APP은???\
타이니두이노 예제 소스코드는 아래 주소에 있다. ^^;\
<https://tiny-circuits.com/learn/tinyshield-ble2>

그리고 Phone단 테스트 APP을 찾아보니.. 저 BLE 보드에 메인이 되는 BLE chip 회사가 또 따로 있네. ^^.

 https://www.bluegiga.com/en-US/products/bluetooth-4.0-modules/ble112-bluetooth--smart-module/documentation/\
![image](/assets/images/posts/5852488/c0028014_5452505376092.png)\
 안드로이드랑 연동할꺼니까 안드로이드 어플을 다운 받았다.\
~~`Bluegiga.apk`~~ *(이글루스 파일 첨부, 서버 종료로 접근 불가)*\
~~`Bluegiga\_Android\_App.zip`~~ *(이글루스 파일 첨부, 서버 종료로 접근 불가)*

우선 추측하기로는 일반 BT 모듈과 마찬가지로 phone이랑은 페어링이 될꺼구.. \
타이니두이노에서는 TX RX 영역에 baud rate만 맞춰서 쓰면 될꺼라 생각한다.\
요건 직접 해보고 다음에 업데이트 하도록 하겠다.