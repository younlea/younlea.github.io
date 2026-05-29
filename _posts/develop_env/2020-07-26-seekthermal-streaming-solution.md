---
title: "seekthermal streaming solution"
excerpt_separator: "<!--more-->"
date: 2020-07-26 05:45:00 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

thermal image streaming solution.

![image](/assets/images/posts/6680662/c0028014_5f1bf2e59990b.png)

we need below package.

HW : respberrypi4, seekthermal J2 or J3 module.

SW :  raspberrypi side

- seekthermal sdk : <https://github.com/younlea/TIC-thermal-imaging-camera>

- v4l2loopback : <https://github.com/umlaeute/v4l2loopback>

- ffmpeg : <https://ffmpeg.org/>

- gstreamer : <https://gstreamer.freedesktop.org/>

- vlc : <https://www.videolan.org/index.ko.html>

windows side

- vlc player : <https://www.videolan.org/index.ko.html>

raspberrypi seeting.

1. download seekthermal sdk

$git clone https://github.com/younlea/TIC-thermal-imaging-camera.git

please check the step on the git page. you can install seekthermal sdk and fine sample source code.

2. download v4l2loopback package and build (sudo apt-get install linux-headers)

$git clone https://github.com/umlaeute/v4l2loopback.git

$cd v4l2loopbakc

$make

$sudo make install

$sudo depmod -a

$sudo modprobe v4l2loopback video\_nr=4

$ls /dev/video4

Now you can test v4l2loopback/example/test  << If this app is correctly working, Now you are ready for next step.

3. launch stream application

build TIC-thermal-imaging-camera/example/seek-stream

$sudo ./seek-stream

![image](/assets/images/posts/6680662/c0028014_5f1d42b333f38.png)

![image](/assets/images/posts/6680662/c0028014_5f1d42be7d471.png)

![image](/assets/images/posts/6680662/c0028014_5f1d42ccdbc46.png)

![image](/assets/images/posts/6680662/c0028014_5f1d42d8b5378.png)

Now we can use streaming node the node(/dev/video4).

let go next step.

4. you can choice encoder and streaming solution( ffmpeg, gstreamer, vlc)

>>>>case of gstreamer

install gstreamer.

apt-get install libgstreamer1.0-0 gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-libav gstreamer1.0-doc gstreamer1.0-tools gstreamer1.0-x gstreamer1.0-alsa gstreamer1.0-gl gstreamer1.0-gtk3 gstreamer1.0-pulseaudio

you can see streming on raspberrypi (In this case, does not dealy via two capture images)

$gst-launch-1.0 v4l2src device=/dev/video4 ! video/x-raw, width=320, height=240 ! autovideosink

![image](/assets/images/posts/6680662/c0028014_5f1d7c6f471c3.png)

**case of using gstreamer (currently not ok)**

Now try streaming to ip (192.168.1.64)

$gst-launch-1.0 v4l2src device=/dev/video4 ! video/x-raw, width=320, height=240 ! videoconvert ! jpegenc ! rtpjpegpay ! udpsink host=192.168.1.64 port=5000

on my mac

$gst-launch-1.0 udpsrc port=5000 ! application/x-rtp,encoding-name=JPEG,payload=26 ! rtpjpegdepay ! jpegdec ! autovideosink

mmmh.. gstreamer have one issue.

**case of ffmpeg**

vlc player using case. : [https://younlea.github.io/develop_env/vlc-server-to-vlc-player/](https://younlea.github.io/develop_env/vlc-server-to-vlc-player/)

ref: gstremer using example :  [https://younlea.github.io/develop_env/gstreamer-사용/](https://younlea.github.io/develop_env/gstreamer-사용/)

v4l2rtspserver : <https://github.com/mpromonet/v4l2rtspserver>

ffmpeg streaming : <https://trac.ffmpeg.org/wiki/StreamingGuide>

\