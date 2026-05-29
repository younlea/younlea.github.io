---
title: "raspberry pi로 TCP/IP server 제작 및 HW control 하기"
excerpt_separator: "<!--more-->"
date: 2013-12-17 00:26:53 +0900
categories:
  - develop_env
tags:
  - develop_env
  - embedded
  - 임베디드

toc : true
toc_sticky : true
---

raspberrypi 개발 환경 셋팅.

login ID : pi

PW : raspberry

내부적으로  IP 및 nfs가 다 되어 있다면 서버를 아래와 같이 mount하여 사용하면 된다.

server : 192.168.0.26

raspberry : 192.168.0.12

mount 방법

sudo mount -t nfs 192.168.0.26:/home/younlea/project /home/pi/test\_code/server\_source/

echo server 및 client 제작

-> 제작 필요.

C++에서 raspberry pi Gpio 컨트롤 방법

<http://hertaville.com/2012/11/18/introduction-to-accessing-the-raspberry-pis-gpio-in-c/>

Shell 에서 setting

|  |  |
| --- | --- |
| 1  2  3  4  5 | pi@raspberrypi ~ $ sudo -i  root@raspberrypi:~# echo "4" > /sys/class/gpio/export  root@raspberrypi:~# echo "out" > /sys/class/gpio/gpio4/direction  root@raspberrypi:~# echo "1" > /sys/class/gpio/gpio4/value  root@raspberrypi:~# echo "0" > /sys/class/gpio/gpio4/value |

android client 개발

<http://myandroidsolutions.blogspot.kr/2012/07/android-tcp-connection-tutorial.html>

C로 raspberry pi gpio 컨트롤 <http://www.rasplay.org/?p=3241>

|  |  |
| --- | --- |
| 1  2  3  4  5  6 | sudo apt-get updatesudo apt-get upgrade  sudo apt-get install git-core  git clone git://git.drogon.net/wiringPi  cd wiringPi  ./build  gpio -v gpio readall |

wiringPi를 사용해서 gpio 4번을 30초간 켜주고 꺼주기를 반복하는 프로그램 sample

|  |  |
| --- | --- |
| 1  2  3  4  5  6  7  8  9  10  11  12  13  14  15  16  17  18  19  20  21  22  23  24 | #include <stdio.h>  #include <wiringPi.h>    #define LED1 7 // BCM\_GPIO 4    int main (void)  {  if (wiringPiSetup () == -1)  return 1 ;    pinMode (LED1, OUTPUT) ;    for (;;)  {  digitalWrite (LED1, 1) ; // On    delay (30000) ; // ms    digitalWrite (LED1, 0) ; // Off    delay (30000) ;  }  } |

build

$ gcc -o gpio gpio.c -lwiringPi

해야 할일 :

**Raspberry pi**

- **raspberry pi에서 running 할 TCP/IP server 제작** (완)

- **raspberry pi 에 wifi 동글 연결하여 ip setting** (완)

--------------------------------------------------------------------------

<http://elinux.org/RPi_USB_Wi-Fi_Adapters> (raspberry pi와 연결되는 USB wifi 동글 목록 및 문제점 목록)

- **Realtek**
  - RTL8188CUS USB-ID 0bda:8176, kernel oops in dmesg and freeze when pulled from USB. (B)

[raspberry pi에 iptime N100mini 연결 방법 공유](http://pklazy.wordpress.com/2013/02/23/%EB%9D%BC%EC%A6%88%EB%B2%A0%EB%A6%AC%ED%8C%8C%EC%9D%B4-n100mini-%EC%84%A4%EC%A0%95/)..

위와같이 해도 안된다면

dhclient wlan0 를 하면 자동으로 DHCP가 IP를 잡아줘야 하는데.. 이것도 안된다.. ㅡㅜ

결국 아래와 같이 HARD Coding을해서 해결...

|  |  |
| --- | --- |
| 1  2  3  4  5  6  7  8  9  10  11  12  13  14  15  16  17  18  19  20  21 | pi@raspberrypi ~ $ **sudo ifconfig wlan0 192.168.0.11 netmask 255.255.255.0 up**  pi@raspberrypi ~ $ ifconfig wlan0  wlan0     Link encap:Ethernet  HWaddr 00:08:9f:da:ad:ae  inet addr:192.168.0.11  Bcast:192.168.0.255  Mask:255.255.255.0  UP BROADCAST MULTICAST  MTU:1500  Metric:1  RX packets:2 errors:0 dropped:2 overruns:0 frame:0  TX packets:2 errors:0 dropped:0 overruns:0 carrier:0  collisions:0 txqueuelen:1000  RX bytes:344 (344.0 B)  TX bytes:288 (288.0 B)    pi@raspberrypi ~ $ ping 192.168.0.1  PING 192.168.0.1 (192.168.0.1) 56(84) bytes of data.  64 bytes from 192.168.0.1: icmp\_req=1 ttl=64 time=1.02 ms  64 bytes from 192.168.0.1: icmp\_req=2 ttl=64 time=0.824 ms  64 bytes from 192.168.0.1: icmp\_req=3 ttl=64 time=0.801 ms  --- 192.168.0.1 ping statistics ---  3 packets transmitted, 3 received, 0% packet loss, time 2002ms  rtt min/avg/max/mdev = 0.801/0.884/1.028/0.104 ms  pi@raspberrypi ~ $ arp -a  ? (192.168.0.2) at 00:1c:c0:7a:88:39 [ether] on eth0  ? (192.168.0.1) at 00:26:66:e1:31:ac [ether] on eth0 |

---------------------------------------------------------------------------

문제는 최초 부팅시 WLAN을 못잡는다.

부팅후 sudo ifup wlan0를 꼭 해줘야 하는데.. /etc/rc.local을 넣어서 하게되면 이상하게 ssh도 막히고 난리다. ㅡㅜ

[[다른 참고]](http://jonghyunkim816.blogspot.kr/2013/10/wifi-usb.html)

---------------------------------------------------------------------------

1.USB wifi 동글 driver 설치

- dmesg | grep usb 검색  (아니면 "lsusb"를 사용해도 된다.)

-> usb 1-1.3: Manufacturer : Realtek   <<확인

- apt-cache search Realtek

-> firmware-realtek 를 확인.

-> sudo apt-get install firmware-realtek

- IP setting

-> sudo iwlist scan | less

-> iwconfig

----> sudo vim /etc/network/interfaces

|  |  |
| --- | --- |
| 1  2  3  4  5  6  7  8  9  10  11 | auto lo    iface lo inet loopback  iface eth0 inet dhcp    allow-hotplug wlan0  auto wlan0    iface wlan0 inet dhcp  wpa-ssid "공유기이름"  wpa-psk "비밀번호" |

----> reboot

---->ifconfig

---------------------------------------------------------------------------

- **raspberry pi 의 gpio control 하여 switch control**

<http://elinux.org/RPi_Low-level_peripherals>

![image](/assets/images/posts/5788732/c0028014_52c6c3794e341.png)

![image](/assets/images/posts/5788732/c0028014_52c6c2c5cfb59.png)

- HW swtich 땜질...- trangister 사용

-IRF520, BC547, 2n3904

![image](/assets/images/posts/5788732/c0028014_52d01653a28a0.png)

Raspberry PI GPIO ------ B에 연결

C를 Switch 위쪽에 연결

E를 스위치 아리에 연결하고 Raspberry PI GND에 연결...

이러면 스위치 컨트롤 잘된다.

**IP time setting (완)**

**Android Phone**

- TCP/IP socket program (1차 버젼 완)

- DNS ip 주소 변환 관련 코드 확인 필요.

- bug fix 필요

초기 부팅시 wifi 자동으로 할당.

/etc/network/interface에서 wifi는 dhcp로 잡아주자.. 모 ap에서 확인하면 되니까...

초기 부팅시 server 자동 실행

/etc/init.d/rc.local 에서..

do\_start 다음에 넣도록...

\