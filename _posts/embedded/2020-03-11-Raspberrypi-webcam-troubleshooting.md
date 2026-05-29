---
title: "Raspberrypi - webcam troubleshooting"
excerpt_separator: "<!--more-->"
date: 2020-03-11 06:45:10 +0900
categories:
  - embedded
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

스트리밍 테스트를 위해 라즈베리에 webcam을 연결했는데 동작을 안한다. 엄...

동작을 안한다는건 간단히 cheese를 실행 했을때 영상을 못 여는 이슈 였다.

기본적으로 아래와 같이 연결하면 /dev  밑에 video12가 잡히는것을 확인할수 있었고.. lsusb를 사용해서도 Device 007에 연결된걸 확인할수 있었습니다.

![image](/assets/images/posts/6616900/c0028014_5e6805023605e.png)

음 그런데. cheese를 실행하면 아래와 같은 에러가 나오더군요..

음.. device가 왜 busy인거지???? 음음음..

![image](/assets/images/posts/6616900/c0028014_5e6805502ff51.png)

이해가 안가서.. 이리저리 구글형한테 물어보다 음.. 이게 어플리케이션 문제일까?? 해서.. 영상 캡쳐 툴을 다시 설치해 봅니다.

sudo apt-get install fswebcam

![image](/assets/images/posts/6616900/c0028014_5e6805b080775.png)

엄 그런데 여기서도 video0가 바쁘다고 나오네요.. 이 녀석은 왜 바쁜거지???

또 구글링을 해봅니다. 결국 카메라를 미리 설치 되어있는 녀석이 사용하고 있다는걸 알게 되었네요 ^^;

<https://www.raspberrypi.org/forums/viewtopic.php?t=104330>

보다보면 motion이라는 녀석이 camera를 열고 있다고 나오네요 ㅡ.ㅡ;

![image](/assets/images/posts/6616900/c0028014_5e6806ae2ee0e.png)

요 녀석을 죽이고 하면 찍히네요. 훗...

![image](/assets/images/posts/6616900/c0028014_5e680734c8893.png)

cheese를 실행하면 아래와 같이 잘 나오네요

![image](/assets/images/posts/6616900/c0028014_5e68084954fd8.png)

훗.. 그럼 문제의 motion은 모하는 녀석일까요??? 궁굼해서 찾아 봤습니다.

억.. 이거 제가 설치했던 webcam streaming soultion이었네요 ㅜㅜ 떠그럴..

지우는건 간단합니다.

sudo apt-get remove motion

ㅜㅜ 자 그럼. 이제 gstreamer를 사용해 봐야것네요. ^^;