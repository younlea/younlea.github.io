---
title: "seekthermal streaming ref"
excerpt_separator: "<!--more-->"
date: 2020-07-16 05:32:22 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

seekthermal

[stcam](https://www.eevblog.com/forum/thermal-imaging/rpi-zero-w-seek-thermal-compact-streaming/) - [link](https://github.com/serj33/stcam)  (http streaming solution)

old version - [link](https://diydrones.com/forum/topics/low-cost-open-source-streaming-thermal-camera-1)  (드론에 달고 하는데 실제로 maverick image를 사용해서 gstreamer 스트리밍함. )

-> maverick - [link](http://goodrobots.github.io/maverick/current/#/modules/vision?id=vision_seek)  [link](https://discuss.ardupilot.org/t/maverick-1-1-4-release-with-new-pi-zero-image/23060)

- [gstreamer 참고](https://dkant.net/2019/05/17/Gstreamer01/)

libseek - [link](https://github.com/zougloub/libseek)  (ffplayer)  [[ffmpeg streaming guild](https://trac.ffmpeg.org/wiki/StreamingGuide)]

libseek-thermal - [link](https://github.com/maartenvds/libseek-thermal)(opencv 이용 draw)

buff ffmpeg rtp - [link](https://lazymankook.tistory.com/9)  (memory buffer를 ffmpeg을 이용해서 인코딩하고 rtp 로 보내는 소스)

flir camera (sample)

v4l2loopback sample - [link](https://diydrones.com/profiles/blogs/thermal-imaging-for-your-drone-on-a-budget)  (드론에 달고 하는데 flir camera와 v4l2 loop back 사용함)

v4l2loopback github - [link](https://github.com/umlaeute/v4l2loopback)

결론은.. 음...

seekwear sdk로 읽어서 kernel 단에서  v4l2loopback으로 영상 버퍼를 쓰고..

이 버퍼를 gstreamer가 읽어서 보내는 걸로 하면 될듯..

이제까지 v4l2loopback을 몰라서 직접 char device driver만들고 있었다는 ㅜㅜ

자 다시 도전.. ^^

엄.. v412loopback build시 아래 에러 발생 ㅡ.ㅡ;

make[1]: \*\*\* /lib/modules/5.4.35-v7+/build: No such file or directory.  Stop.

또 빌드 에러네.. 아웅.. ㅡ.ㅡ;

컥.. ㅡ.ㅡ; build에 ln만 하면 되나??

![image](/assets/images/posts/6676460/c0028014_5f10b87d91529.png)

움 header file이 없는데 ㅡ.ㅡ;

```
sudo apt-get install linux-headers-$(uname -r)
```

위와 같이 하면 설치가 되야 하는데 없다고 하네 ㅡ.ㅡ 강제로 버젼을 찾아서 받고 ln해봐야 겠다 ㅡ.ㅡ 아웅.

일단 검색은

$apt-cache search linux-headers

$sudo apt update

일단 오기로 해당 파일들을 찾아본다.

<https://pkgs.org/download/kernel-headers>

엄. 다시 시작. ㅡ.ㅡ;

그냥 새로 설치하고.. linux kernel 4.9.118-v7+

여기서 설치해 보니.. 엄..

v4l2loopback 설치 및 install

git clone https://github.com/umlaeute/v4l2loopback.git

make

sudo make install

sudo depmod -a     <<혹시나 이거 안하면 [modprobe: FATAL: Module v4l2loopback not found in directory /lib/modules/4.19.118-v7+] 에러 납니다..

sudo modprobe v4l2loopback

자.. 이제 /dev/video0, /dev/video1이 생깁니다. ㅋㅋ

\