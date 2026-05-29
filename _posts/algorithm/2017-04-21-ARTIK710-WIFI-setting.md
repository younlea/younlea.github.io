---
title: "ARTIK710 WIFI setting"
excerpt_separator: "<!--more-->"
date: 2017-04-21 16:37:53 +0900
categories:
  - algorithm
tags:
  - algorithm
  - 알고리즘

toc : true
toc_sticky : true
---

**접속 가능한 wifi AP를 스켄합니다.**

**[root@localhost ~]#iwlist wlan0 scanning**

결과..

wlan0     Scan completed :

Cell 01 - Address: 64:E5:99:CD:30:64

Channel:3

Frequency:2.422 GHz (Channel 3)

Quality=59/70  Signal level=-51 dBm

Encryption key:on

ESSID:"test1111"

Bit Rates:1 Mb/s; 2 Mb/s; 5.5 Mb/s; 11 Mb/s; 6 Mb/s

9 Mb/s; 12 Mb/s; 18 Mb/s

Bit Rates:24 Mb/s; 36 Mb/s; 48 Mb/s; 54 Mb/s

Mode:Master

Extra:tsf=00000004132300ac

Extra: Last beacon: 10ms ago

IE: Unknown: 000569676E6973

IE: Unknown: 010882848B960C121824

IE: Unknown: 030103

IE: Unknown: 2A0100

위 내용중 ESSID를 확인하고.. 해당 AP의 password를 확인합니다.

**/etc/wpa\_supplicant/wpa\_supplicant.conf 파일 열어서 아래 부분 입력을 합니다.**

----------------------------------

network={

ssid="test1111"

psk="password"

}

--------------------------------

**\**

**IP를 할당 받습니다.**

[root@localhost wpa\_supplicant]# **dhclient wlan0**

**할당되었는지 확인을 합니다.**

[root@localhost wpa\_supplicant]# **ifconfig wlan0**

wlan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500

inet 192.168.0.15  netmask 255.255.255.0  broadcast 192.168.0.255

inet6 fe80::722c:1fff:fe23:e29d  prefixlen 64  scopeid 0x20<link>

ether 70:2c:1f:23:e2:9d  txqueuelen 1000  (Ethernet)

RX packets 119  bytes 28152 (27.4 KiB)

RX errors 0  dropped 185  overruns 0  frame 0

TX packets 39  bytes 5624 (5.4 KiB)

TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

**접속되는지 확인을 합니다.**

[root@localhost wpa\_supplicant]# **ping www.google.com**

PING www.google.com (216.58.199.100) 56(84) bytes of data.

64 bytes from hkg07s22-in-f4.1e100.net (216.58.199.100): icmp\_seq=1 ttl=44 time=42.1 ms

연결 확인 끝...

참 쉽죠잉... ^^;