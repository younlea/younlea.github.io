---
title: "Raspberry Pi USB to Serial setting"
excerpt_separator: "<!--more-->"
date: 2016-10-04 23:51:26 +0900
categories:
  - embedded
tags:
  - develop_env
  - embedded
  - 임베디드

toc : true
toc_sticky : true
---

USB to Serial 연결 방법은 아래와 같습니다.  헌데 아래 링크의 경우는 라즈베리파이 초기 모델입니다.

<http://www.rasplay.org/?p=1617>

최근 Raspberrypi 3관련 회로구성은 아래 링크와 같은데 이 부분에서 핀 배치도를 보면 UART관련 핀 배치는 위 링크와 동일한걸 볼수 있습니다.

<http://stackoverflow.com/questions/38813679/raspberry-pi3-c-serial-communication-not-working-properly-raspberry-pi-was-w>

raspberry pi gpio 비교

<http://www.raspberrypi-spy.co.uk/2012/06/simple-guide-to-the-rpi-gpio-header-and-pins/>

라즈베리 파이 회로 연결을 하고 나면 연결할 PC에 드라이버를 설치 해야 합니다.

대표 적인 USB Serial 관련 드라이버는 아래 두개와 같습니다

FTDI - <http://www.ftdichip.com/FTDrivers.htm>  - [driver download link](http://www.ftdichip.com/Drivers/D2XX.htm)

Prolific - [windows driver](http://www.prolific.com.tw/US/ShowProduct.aspx?p_id=225&pcid=41) - [MAX OS driver](http://www.prolific.com.tw/US/ShowProduct.aspx?p_id=229&pcid=41)

이제 위와 같이 설치하고 Windows에서는 putty를 사용해서 연결을 하면 됩니다.

여기서 MAC에서 연결해서 사용하려면.. 또.. 다른 삽질을 해야 합니다. ^^;

mac에서 usb to serial 사용하기

<http://www.nexpert.net/463> (드라이버는 해당 링크에 있는걸 쓰면 안됩니다. 아래 링크를 이용해서 받으시기 바랍니다.)

-> <http://www.prolific.com.tw/US/ShowProduct.aspx?p_id=229&pcid=41>

MAC에서 동작을 간단히 정리하면..

1. Driver 설치 및 rebooting

2. 1번이 완료 되면 자연적으로 network setting 들어가면 USB Serial이 생깁니다. (안생기면 드라이버 설치 다시 하시길)

열어서 셋팅은 -> null modem, 115200

3. 실제 연결은 CMD 창에서 screen을 사용해서 연결합니다.

ls /dev | grep serial  --> cu.usbserial 검색이 됩니다.

실제 연결 : screen /dev/cu.usbserial

USB to Serial 연결이 완료 되면.. 이제 wifi를 shell을 통해서 셋팅을 해야 합니다.

WIFI setting

<https://www.maker.io/en/blogs/raspberry-pi-3-how-to-configure-wi-fi-and-bluetooth/03fcd2a252914350938d8c5471cf3b63>

정리하면..

1. 접속 가능한 wifi AP를 스켄합니다.

iwlist wlan0 scan

2.접속할 AP를 셋팅합니다.

/etc/wpa\_supplicant/wpa\_supplicant.conf 파일 열어서 아래 부분 입력을 합니다.

----------------------------------

network={

ssid="The\_ESSID\_from\_earlier"

psk="Your\_wifi\_password"

}

----------------------------------

The\_ESSID\_from\_earlier  << 연결할 AP 이름

Your\_wifi\_password << AP 비밀번호

\