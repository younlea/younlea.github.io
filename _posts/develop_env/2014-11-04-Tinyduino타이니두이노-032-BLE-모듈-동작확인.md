---
title: "Tinyduino(타이니두이노) - 03-2. BLE 모듈 동작확인"
excerpt_separator: "<!--more-->"
date: 2014-11-04 00:39:26 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

Tinyduino 개발환경을 셋팅하고.. 이제 BLE Module 셋팅 테스트를 해보려고 합니다.

최초 생각은 BLE 모듈에 전원만 공급해 주면 되는줄 알았는데 안되더군요.. ㅜㅜ

자.. 다시 타이니두이노 싸이트를 찾아보니 친절하게 아래와 같이 가이드를 해주고 있더군요

https://tiny-circuits.com/learn/tinyshield-ble2\
~~`BGLib\_stub\_slave\_rev2-2.zip`~~ *(이글루스 파일 첨부, 서버 종료로 접근 불가)*

위 파일을 받아서 아두이노 빌드환경에서 빌드후 다운로드를 하면 BLE 셋팅이 완료가 됩니다.

그리고 Android Phone에서 아래 파일을 설치후 BLE를 검색하면 아래 그림처럼 확인이 가능하게 됩니다.\
~~`Bluegiga.zip`~~ *(이글루스 파일 첨부, 서버 종료로 접근 불가)*\
![image](/assets/images/posts/5853096/c0028014_5457a222c6bab.png)