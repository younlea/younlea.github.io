---
title: "Tinyduino(타이니두이노) - 03-5. bluetoothchat program연결 with BT"
excerpt_separator: "<!--more-->"
date: 2014-11-17 23:20:03 +0900
categories:
  - dataScience
tags:
  - dataScience
  - datascience
  - 데이터

toc : true
toc_sticky : true
---

BLE를 사용해서 SPP와 유사한 기능을 구현하기로 했던것은 일단 일정상 뒤로 미루기로 했다.

메가솔루션

http://www.mechasolution.com/shop/goods/goods\_view.php?goodsno=466&category=001011

타이니두이노 본래 싸이트 참조

https://tiny-circuits.com/learn/tinyshield-bt

이전까지 쓰던 3세대 BT 모듈이다..

bluetooth chat program을 Phone에 올리고 아래 코드를 타이니두이노에 쓰게 되면..

모.. 에코 기능을 할것 같다..

|  |  |
| --- | --- |
| 1234567891011121314 | ```  void setup()  {  Serial.begin(57600);  }    void loop()  {  if (Serial.available())  {  Serial.print("The character typed is: ");  Serial.write(Serial.read());  Serial.println("");  }  } ``` |

일단 해보자구 ^^;

타이니두이노 보드, usb 확장보드, BT 보드를 연결해서 USB로 연결해서 바이너리 다운로드시 아래와 같은 문제가 발생한다

![image](/assets/images/posts/5854917/c0028014_546a04d15e9fb.png)

BT 모듈과 USB 모듈이 둘다 동일 serial포트를 써서 생기는 문제인걸로..  BT을 우선 빼고 프로그램을 해보자.

잘된다.. ^^;

![image](/assets/images/posts/5854917/c0028014_546a0510f2a76.png)

우선 만들었던 안드로이드 툴을 실행한다.

![image](/assets/images/posts/5854917/c0028014_546a072e121f1.png)

메뉴키를 눌러서 디바이스를 찾는다.

![image](/assets/images/posts/5854917/c0028014_546a07445b16c.png)

스켄 디바이스를 클릭하고

![image](/assets/images/posts/5854917/c0028014_546a074b2fb35.png)

아래와 같이 디바이스를 찾고

![image](/assets/images/posts/5854917/c0028014_546a07655ba6b.png)

커넥트를 눌르면... 연결을 시도한다.. ^^;

![image](/assets/images/posts/5854917/c0028014_546a0774f0f86.png)

연결할꺼냐고 묻고..

![image](/assets/images/posts/5854917/c0028014_546a078abb6ed.png)

살짝쿵 연결을 한다.

![image](/assets/images/posts/5854917/c0028014_546a079621485.png)

연결이후 메세지를 보내면 답변은 오는데.. 잉.. 저거 모지.. ㅡ.ㅡ; 자.. 이건 바로 분석들어간다 ^^;

![image](/assets/images/posts/5854917/c0028014_546a07a347bd1.png)

아마 baud rate 차이에서 생기는 문제로 확인해 보자..

일단 신호가 간다고 생각하고 바로 다른 작업에 들어가 보자. ^^; (분석은 좀 나중에.. ㅡ.ㅡ;

\