---
title: "Windows에서 ADB 연결을 CMD로 할 경우 글자 깨지거나 탭이 먹지 않을 경우 해결책..."
excerpt_separator: "<!--more-->"
date: 2016-07-05 14:12:35 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

MobaXterm을 사용할 경우

---------------------------------------------------------------------------------

Windows/ 아래에 아래 파일 카피

adb.exe

AdbWinApi.dll

AdbWinUsbApi.dll

MobaXterm open하고 adb shell 하면 cmd창 깨지지 않음.  (단 탭이 먹지는 않음)

---------------------------------------------------------------------------------

Putty로 ADB 연결하는 방법.

첨부한 putty를 사용 하고 adb를 선택후 transport-usb 로 접속하는 곳을 셋팅하면 사용 가능.

~~`putty.zip`~~ *(이글루스 파일 첨부, 서버 종료로 접근 불가)*

참조 : http://mungi.kr/169

로깅도 putty옵션에서 주면 편하게 볼수 있다.

Actually the Android Debug Bridge has a terminal connection feature (roughly speaking), which will be enabled after you connect to the adb server in "0006shell:" mode. You can actually use the putty to connect to this interface always, by setting the following things:

- Turn off line discipline in settings

- Use RAW mode to connect to localhost:5037

- Enter "0012transport-usb" (without quotes)

- Enter "0006shell:" (without quotes)

Now you've got a full fledged connection to your device. The main drawback is that it's tedious to repeat the above all the time, so I've made some modifications to the putty binary that adds a new type of connection, called "Adb"

To use the enhanced putty:

- Select Adb from the connection type list

- Enter "transport-usb" in the host (or any other connection string, check the adb socket interface documentation if you need something else than connecting via usb)

- Enter 5037 as port, if it's not already set there.

- Connect and enjoy (you might also save this connection, so next time you only have to double-click on the settings)

DL and source: http://github.com/sztupy/adbputty/downloads

\