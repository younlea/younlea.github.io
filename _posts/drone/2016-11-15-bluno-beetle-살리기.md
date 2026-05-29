---
title: "bluno beetle 살리기.."
excerpt_separator: "<!--more-->"
date: 2016-11-15 14:37:44 +0900
categories:
  - drone
tags:
  - develop_env
  - embedded
  - 임베디드

toc : true
toc_sticky : true
---

[https://younlea.github.io/Embedded/nanowii-생명-불어-넣기/](https://younlea.github.io/Embedded/nanowii-생명-불어-넣기/)

https://www.devicemart.co.kr/1171449

https://www.dfrobot.com/wiki/index.php/Bluno\_Beetle\_SKU:DFR0339

isp 위치

![image](/assets/images/posts/6066515/c0028014_582a9ef7c4064.png)

필요한 MOSI, MISO, RESET, SCK 찾았으니.. 살려보자.

![image](/assets/images/posts/6066515/c0028014_5831c0a30de7c.png)

두개를 잘 연결하고..

펌웨어 downloader : http://www.fischl.de/usbasp/

~~`KhazamaAVRProgrammer.rar`~~ *(이글루스 파일 첨부, 서버 종료로 접근 불가)*

bootloader는 그냥 아두이노펌웨어에서 골라서 쓰면 되것다

\Arduino\hardware\arduino\bootloaders\atmega\

https://www.dfrobot.com/wiki/index.php/Bluno\_SKU:DFR0267#Update\_BLE\_Firmware\_on\_Bluno.EF.BC.88AT.2BVERSION\_to\_check\_the\_version.EF.BC.89

\