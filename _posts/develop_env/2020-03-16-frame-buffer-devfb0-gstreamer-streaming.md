---
title: "frame buffer (/dev/fb0) - gstreamer streaming"
excerpt_separator: "<!--more-->"
date: 2020-03-16 09:56:55 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

source를 아래와 같이 넣고 도전.

gst-launch-1.0 -v filesrc location=/dev/fb

gst-launch-1.0 -v multifilesrc location=/dev/fb0

gst-launch-1.0 multifilesrc device=/dev/fb0 ! video/x-raw, width=640, height=480 ! videoconvert ! jpegenc ! rtpjpegpay ! udpsink host=192.168.1.60 port=5000

훔 입력이 안맞나 보다 ㅜㅜ

![image](/assets/images/posts/6619406/c0028014_5e73d667c74c0.png)

 gst-launch-1.0 multifilesrc location=/dev/fb0 ! video/x-raw, width=640, height=480 ! videoconvert ! jpegenc ! rtpjpegpay ! udpsink host=192.168.1.60 port=5000

![image](/assets/images/posts/6619406/c0028014_5e73d731db78d.png)

움.. 싸이즈가 안맞나 보다.. ㅡ.ㅡ;

ㅋ.. 하나씩 하나씩 가는구나~~~

gst-launch-1.0 multifilesrc location=/dev/fb0 ! video/x-raw, width=320, height=240 ! videoconvert ! jpegenc ! rtpjpegpay ! udpsink host=192.168.1.60 port=5000

![image](/assets/images/posts/6619406/c0028014_5e73d8f2edf0e.png)

엄.. format과 framerate를 추가 했는데.. 음..

gst-launch-1.0 multifilesrc location=/dev/fb0 ! video/x-raw, width=320, height=240, format={RGBA}, framerate=[1/30] ! videoconvert ! jpegenc ! rtpjpegpay ! udpsink host=192.168.1.60 port=5000

![image](/assets/images/posts/6619406/c0028014_5e752b726a7e7.png)

32bit RGB는  RGBA 나 ABGR 로 집어 넣구..

H.264 로 전송하면 좋다고 함 .

<https://gstreamer.freedesktop.org/documentation/rawparse/rawvideoparse.html?gi-language=c>

gst-launch-1.0 multifilesrc location=/dev/fb0 ! video/x-raw, width=320, height=240, format={RGBA}, framerate=[0/1] ! videoconvert ! jpegenc ! rtpjpegpay ! udpsink host=192.168.1.60 port=5000

훔 faramrate를 바꿨는데 여전히 에러는 동일하게나는건가?

pi@raspberrypi:~ $ gst-launch-1.0 multifilesrc location=/dev/fb0 ! video/x-raw, width=320, height=240, format={RGBA},framerate=[0/1] ! videoconvert ! jpegenc ! rtpjpegpay ! udpsink host=192.168.1.60 port=5000

WARNING: erroneous pipeline: could not parse caps "video/x-raw, width=320, height=240, format={RGBA}, framerate=[0/1]"

[raw image 확인](https://m.blog.naver.com/PostView.nhn?blogId=chandong83&logNo=221274125929&proxyReferer=https%3A%2F%2Fwww.google.com%2F)

mplayer설치.. 및 테스트 - 엄.. 이미지가 깨지는데.. 그리고 frame buffer 0 가 ... 아무래도. 화면인것 같다. ㅋ..

pi@raspberrypi:~mplayer -demuxer rawvideo -rawvideo w=320:h=240:format=RGBA /dev/fb0 -loop 0

![image](/assets/images/posts/6619406/c0028014_5e82553f481ee.png)

ㅋㅋㅋㅋ 이거 보니까 /dev/fb0에 뿌리고 있는데.. 거기가.. 화면이네 ㅡㅡ;

mplayer로 그리는 RGB데이터가.. 깨지는건.. 버퍼 영역을 잘못 잡아서 인것 같구.. ㅜㅜ

코드부터 다시 보자..  ^^ fb0가 아니라 새로운 영역에 쓰도록 바꾸면 될듯 ^^;

![image](/assets/images/posts/6619406/c0028014_5e82547722d59.png)

gstreamer debug

https://embeddedartistry.com/blog/2018/02/22/generating-gstreamer-pipeline-graphs/

https://gstreamer.freedesktop.org/documentation/tutorials/basic/debugging-tools.html?gi-language=c

debug결과물

1. driver 생성 - misc\_register 참고. - blockdevice driver를 만들면 됨. -[참고](https://kkangstory.tistory.com/entry/LDD)

- [참고2](http://forum.falinux.com/zbxe/index.php?mid=device_driver&page=4) [참고3](https://wiki.kldp.org/Translations/html/The_Linux_Kernel-KLDP/tlk8.html)

2. v4l2 usb driver생성.

3. [sample](https://android.googlesource.com/kernel/common/+/19cada644d55214b6d08e4a8b2345eac1c479167/drivers/staging/android/logger.c) << 안드로이드 로그 생성 관련 블록디바이스 드라이버 사용 코드

4. [ram disk sampel source code](https://linux-kernel-labs.github.io/refs/heads/master/labs/block_device_drivers.html#)  - [sample](https://github.com/martinezjavier/ldd3/blob/master/sbull/sbull.c)   << test

5. [driver sample code](https://gamdekong.tistory.com/127)  << 한국 개발자 간단 설명

6. [block device driver](https://www.apriorit.com/dev-blog/195-simple-driver-for-linux-os)  << 블록 디바이스 드라이버에 대해서 전체적인 설명이 영어로 잘 되어 있음

7. [simple block device driver example](https://lwn.net/Articles/58720/), [same source code](https://blog.superpat.com/2010/05/04/a-simple-block-driver-for-linux-kernel-2-6-31/)   << test

make file을 추가해준다.

```
obj-m := sbd.oKDIR := /lib/modules/$(shell uname -r)/buildPWD := $(shell pwd)default:$(MAKE) -C $(KDIR) SUBDIRS=$(PWD) modules
```

-> build할때 linux/module.h를 못찾음.. [참고](https://ashesmin.tistory.com/7)

[# sudo apt-get update && sudo apt-get install linux-headers-`uname -r`  ]

-> 설치 에러 남 ㅡ.ㅡ;

![image](/assets/images/posts/6619406/c0028014_5eb9b77ca1ea7.png)

엄 ..  stackoverflow에서 커널 업데이트하면 된다고해서 ㅡ.ㅡ; [라즈베리 업데이트](https://www.raspberrypi.org/documentation/linux/kernel/updating.md) 함.

$sudo rpi-update

![image](/assets/images/posts/6619406/c0028014_5eb9bba9a73e1.png)

\