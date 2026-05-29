---
title: "coral dev board 셋팅"
excerpt_separator: "<!--more-->"
date: 2022-05-26 08:13:51 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

coral usb board의 경우 라즈베리 파이에 연결해서 쓰다보니 UI 인터페이스가 편했는데

Coral dev board의 경우 mdt라는 툴을 사용해서 (마치 adb나 sdb 같은)하는 거라 익숙하지 않네요.

그리고 바이너리도 오래전 방식을 채용해서 인터널 롬에 라이트 하는 방식이라 라즈베리파이에 익숙한 사람은 처음에 어려울수 있겠다라구요..

일단 windows ubuntu에서 설치하고 사용하는 방법은 아래 블로그에 정리가 잘 되어 있다.

<https://blog.naver.com/PostView.naver?blogId=elepartsblog&logNo=222223086526>

MDT : mendel develop tool : <https://coral.ai/docs/dev-board/mdt/>

아래는 우분투 환경에서 연결한걸 정리한 블로그이다.

[우분투 환경에서 연결 정리 링크](https://goodtogreate.tistory.com/entry/Coral-Dev-Board-Google-Edge-TPU-%EC%84%A4%EC%A0%95-%EB%B0%8F-%EC%82%AC%EC%9A%A9%ED%9B%84%EA%B8%B0)

[외국 아저씨가 정리한 블로그](https://aallan.medium.com/hands-on-with-the-coral-dev-board-adbcc317b6af)도 있다.

움 그런데.. 화면 연결하고 키보드로 하는 가이드는 별루 없네요. 요것 확인해 봐서 추가할께요.

화면 연결하고 하는것도 보면 terminal을 열고 커멘드를 치면 되긴 한다.

알게된 사실..

따로 크롬 같은 패키지가 깔려있지 않다.

대부분 아래 프로세스로 진행한다.

부팅할수 있도록 바이너리르 다운로드 한다.

mdt를 이용하여 ssh 연결을 하는데.

일단 USB + mdt 를 이용하여 연결하고 wifi setting 을 한다.

wifi setting이 끝나면 wifi를 이용해서 mdt를 연결 하고 터미널에서 코딩 및 러닝을 돌리고

결과는 local webview를 이용하여 확인한다.

그러니까 board에 ip가 셋팅이 되면 확인하구..

이후에 보드에서 edgetpu\_demo --stream을 실행한후에

동일 AP에 연결된 PC 브라우져에서 ip:4664를 넣어주면 실제 예제가 동작하는걸 확인할 수 있다.

흠냐. 어떻게 보면 러닝과 결과를 보는데 최적화 되어 있는듯.. 결국 모니터는 필요 없는것 같다 ^^;

\