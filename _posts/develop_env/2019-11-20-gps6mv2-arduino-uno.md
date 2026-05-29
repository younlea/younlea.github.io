---
title: "gps6mv2 arduino uno"
excerpt_separator: "<!--more-->"
date: 2019-11-20 23:37:50 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

움.. UNO에 gps6mv2를 연결해서 gps값을 읽는걸 하고 싶어하는 분들이 있어서.. 함 해보았습니다. 6^^;

GPS 모듈 구매처 : [링크](http://www.devicemart.co.kr/goods/view?no=1321968)

자세한 설명은 아래 링크에서 참고하세요.. (이쁘게 쓰기기 힘들어서 ㅡ.ㅡ;)

[링크](https://devicemart.blogspot.com/2019/05/neo-6m-gps-gy-gps6mv2.html)

요는. .아래와 같습니다.

GPS 관련 library를 받습니다. [Link](https://github.com/mikalhart/TinyGPS)

아두이노에서 라이브러리 추가(스케치->라이브러리 포함하기->zip라이브러리 추가) 하고

라이브러리 업데이트(툴-> 라이브러리 관리) 합니다.

그리고 파일->예제->tiny GPS-master->simple-test 열어 주고...

setup()에 있는 속도 설정을 ss.begin(9600)으로 수정..

---------------------------------------------------------

void setup()

{

Serial.begin(115200);

Serial.print("Testing TinyGPS library v. "); Serial.println(TinyGPS::library\_version());

Serial.println("by Mikal Hart");

Serial.println();

Serial.println("Sats HDOP Latitude  Longitude  Fix  Date       Time     Date Alt    Course Speed Card  Distance Course Card  Chars Sentences Checksum");

Serial.println("          (deg)     (deg)      Age                      Age  (m)    --- from GPS ----  ---- to London  ----  RX    RX        Fail");

Serial.println("-------------------------------------------------------------------------------------------------------------------------------------");

ss.begin(**9600**);

}

-----------------------------------------------------------------

하드웨어적으로는 GPS TX <-> 아두이노 4, GPS RX <-> 아두이노3번 핀으로 연결.

VCC, GND도연결해야 합니다.

실행시키면.. 각종 정보가 ,serial로 나와야 하는데.. 움 값이 안나오네요 ..

GPS 보드에 전원을 인가하면 불이 들어와야하는데 이건 확인해 봐야겠네요.

\