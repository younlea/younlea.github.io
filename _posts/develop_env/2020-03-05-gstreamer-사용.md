---
title: "gstreamer  사용"
excerpt_separator: "<!--more-->"
date: 2020-03-05 15:17:32 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

Gstreamer install

<https://gstreamer.freedesktop.org/documentation/installing/index.html>

라즈베리에서 설치해서 아래 스크립트로 했는데. 세개가 설치가 안된다 ㅡ.ㅡ

![image](/assets/images/posts/6612099/c0028014_5e680c47ba934.png)

gstreamer1.0-gl gstreamer1.0-gtk3 gstreamer1.0-qt5 << 요건 빼주고 설치.

1. camera를 열어서 화면에서 보여주기.

gst-launch-1.0 v4l2src device=/dev/video0 ! video/x-raw, width=640, height=480 ! autovideosink

라즈베리파이에서 실행하면 아래처럼 나온다 ^^ ;

![image](/assets/images/posts/6612099/c0028014_5e680cf653156.png)

2. 고정IP로 보내고 받아서 보여주는 방법.

영상 올려주는 부분 (raspberrypi 에서 구현함)

$ gst-launch-1.0 v4l2src device=/dev/video0 ! video/x-raw, width=640, height=480 ! videoconvert ! jpegenc ! rtpjpegpay ! udpsink host=xxx.xxx.xxx.xxx port=5000

(IP주소는 받는 쪽 주소를 넣어준다)

영상 받는 부분

$gst-launch-1.0 udpsrc port=5000 ! application/x-rtp,encoding-name=JPEG,payload=26 ! rtpjpegdepay ! jpegdec ! autovideosink

-> Nvidia jetson nano Ubuntu PC에서 동작 확인 완료

-> MAC book에서 받으려면 gst package를 설치해야 한다. 그런데 설치가 안되네 ㅡ.ㅡ

%brew install gstreamer gst-libav gst-plugins-ugly gst-plugins-base gst-plugins-bad gst-plugins-good

![image](/assets/images/posts/6612099/c0028014_5e680fa803e44.png)

-> 이건 sudo xcodebuild -license 치고 스페이스 친후에 agree치면 된다.

이후 다시 설치하면 아래와 같이 설치하다 에러 남. ㅜㅜ 움.. 설치 이슈인것 같은데..

![image](/assets/images/posts/6612099/c0028014_5e6b7f7279a18.png)

크. 그냥 설치하면 되나보다.

<https://gstreamer.freedesktop.org/documentation/installing/on-mac-osx.html?gi-language=c>

<https://gstreamer.freedesktop.org/documentation/deploying/mac-osx.html?gi-language=c>

그냥.. 루비로 다운로드 가능하네 ㅡ.ㅡ; 아래 링크 사용하기로..

<http://macappstore.org/gstreamer/>

1. cmd 창을 염.

2. ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" < /dev/null 2> /dev/null

3. brew install gstreamer

설치가 끝나면 실행해 보자구~~~

엄.. brew install gst-libav gst-plugins-ugly gst-plugins-base gst-plugins-bad gst-plugins-good

아까 설치 안했던거 마져 깔고..

gst-launch-1.0 udpsrc port=5000 ! application/x-rtp,encoding-name=JPEG,payload=26 ! rtpjpegdepay ! jpegdec ! autovideosink

성공..

![image](/assets/images/posts/6612099/c0028014_5e6b928e78bde.png)

참고

<https://medium.com/@endland/gstreamer-%EC%8B%A4%EC%8B%9C%EA%B0%84-streaming-f7e6f4608768>

<https://yujuwon.tistory.com/entry/GStreamer%EB%9E%80>

20200305 - frame buffer node 떨군걸 1.camera 열기 방법으로 열리는지 확인.

아니라면 해당 데이터를 영상 data로 떨구는걸 확인

\