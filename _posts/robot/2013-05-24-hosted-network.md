---
title: "hosted network"
excerpt_separator: "<!--more-->"
date: 2013-05-24 01:29:55 +0900
categories:
  - robot
tags:
  - robot
  - 로봇

toc : true
toc_sticky : true
---

google 검색 msdn hosted network

cmd, api

http://msdn.microsoft.com/en-us/library/windows/desktop/dd815243(v=vs.85).aspx

cmd로 실행하기.

cmd를 관리자 권한으로 실행.

netsh wlan set hostednetwork ssid=test key=12345678 (이후 무선 네트워크 연결 2가 새로 생긴다. )

netsh wlan start hostednetwork  (이렇게 하면 Wifi AP가 만들어져서 사용 가능해 진다.)

간헐적으로 안되는 PC가 있는데 아래 명령으로 확인 가능하다.

netsh wlan show drivers

호스트된 네트워크 지원 : 아니오   <<= 제대로 된 드라이버가 없거나 지원 안됨.

참고 주소

http://circlash.tistory.com/666

http://mudaebbo.tistory.com/290

http://mungi.tistory.com/209

관련 기능의 프로그램 : connectify