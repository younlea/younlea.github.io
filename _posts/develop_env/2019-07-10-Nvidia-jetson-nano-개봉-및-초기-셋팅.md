---
title: "Nvidia jetson nano 개봉 및 초기 셋팅"
excerpt_separator: "<!--more-->"
date: 2019-07-10 11:32:11 +0900
categories:
  - develop_env
tags:
  - develop_env
  - embedded
  - 임베디드

toc : true
toc_sticky : true
---

어쩌다가 머신러닝을 해보려구 nvidia jetson nano를 구매했네요. 가격은 99$라고 하지만...

최저가는 중국 seeedstudio.com 이지만 배송료와 배송 시간을 고려했을때 그냥 국내에서 사는게 더 이익이라 ㅡ.ㅡ;

그냥 아래에서 구매 했음.. (메모리 카드나 wifi 모듈은 집에 굴러다녀서 그냥 이 제품만 구매함. ㅡ.ㅡ;)

[구매 링크](http://mdsshop.co.kr/product/detail.html?product_no=134&cate_no=1&display_group=6)

![image](/assets/images/posts/6507181/c0028014_5d253a5136823.png)

자.. 무려 2일만에 집으로 배송이 왔는데 다른거 하느라 못하고 일주일이 지나지 않은 시점에서 기본 바이너리 올려 보려고 합니다. ^^;

간단한 한글 스펙등은 아래 한글 링크에서 보면 되고.. 개발에 관한건 일단 영문 개발 링크에서 확인해 봤습니다.

한글 링크 : <https://www.nvidia.com/ko-kr/autonomous-machines/embedded-systems/jetson-nano/>

영문 개발 링크 : <https://developer.nvidia.com/embedded-computing>

<https://developer.nvidia.com/embedded/jetson-nano-developer-kit>

1단계. 일단 부팅이 되야 하니.. 가지고 있는 32GB sdcard 에 부팅 파일을 설치 합니다.

<https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit#write>

![image](/assets/images/posts/6507181/c0028014_5d253d63c3d13.png)

모 하라는데로 하면 됩니다. ^^; 1번 링크를 눌러서 다운로드 하고 2번, 자신의 PC에 맞는 설치 방법을 찾아 들어갑니다.

라즈베리파이 설치랑 크게 다르지 않네요 ㅡ.ㅡ; MAC 이라 etcher라는 툴을 사용해서 설치하고 있습니다.

![image](/assets/images/posts/6507181/c0028014_5d2540ca34832.png)

이미지가 다운로드 받은 압축 파일이 5G정도인데 설치이미지는 12.88GB네요.. 32GB짜리로 설치하고 있는데 필요하면 64GB로 바꿔야 할듯하네요 ^^;

![image](/assets/images/posts/6507181/c0028014_5d254e0d18dfa.jpg)

![image](/assets/images/posts/6507181/c0028014_5d254e1c29269.jpg)

![image](/assets/images/posts/6507181/c0028014_5d254e2565577.jpg)

부팅은 잘되네요. ^^

추가로 필요했던 디바이스: LCD, USB 전원, wifi dongle, keyboard, mouse

다음으로 모할지는 아래 링크에 있는것들을 따라 하면 될듯 합니다.

<https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit#next>

<https://courses.nvidia.com/courses/course-v1:DLI+C-RX-02+V1/about>

베란다에 놓구 집에서 하려니 VNC를 설치해야해서 설치 해보는중. (참고로 wifi로 연결한 ssh가 자꾸 끊기는 이슈가 있다 ㅜㅜ)

참고 : <https://tattler.tistory.com/210>

\