---
title: "arduino USB name 바꾸기~"
excerpt_separator: "<!--more-->"
date: 2015-09-21 13:34:47 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

[bootloader source code](https://github.com/arduino/Arduino/tree/master/hardware/arduino/avr/bootloaders)

수정 source code 위치

C:\Program Files (x86)\Arduino\hardware\arduino\avr

Descriptors.c (avr\bootloaders\caterina):.UnicodeString          = L"Arduino Leonardo"

Boards.txt (avr):leonardo.pid.0=0x0036

Boards.txt (avr):leonardo.pid.2=0x0036

Descriptors.c (avr\bootloaders\caterina):#if DEVICE\_PID == 0x0036

Descriptors.c (avr\bootloaders\caterina-arduino\_robot):#if DEVICE\_PID == 0x0036

위 이름을 바꾼후에 boot loader를 build(compile)를 해야 한다.

[bootloader\_build](http://angryelectron.com/how-to-update-the-bootloader-on-arduino-pro-mini-328/) -- linux에서 빌드하는 방식에 대해서 확인 가능.

[build\_link\_1](http://krazatchu.ca/2012/04/01/how-to-compiling-the-arduino-bootloader/)

[build\_link\_2](https://ariverpad.wordpress.com/2012/02/24/recompiling-and-uploading-the-arduino-pro-mini-3-3v-bootloader/)

[build\_link\_3](http://angryelectron.com/stk500-arduino-bootloader/)

/source/Arduino/hardware/arduino/avr/boards.txt

```
##############################################################
 
leonardo.name=Arduino Leonardo
leonardo.vid.0=0x2341
leonardo.pid.0=0x0036
leonardo.vid.1=0x2341
leonardo.pid.1=0x8036
leonardo.vid.2=0x2A03
leonardo.pid.2=0x0036
leonardo.vid.3=0x2A03
leonardo.pid.3=0x8036
 
leonardo.upload.tool=avrdude
leonardo.upload.protocol=avr109
leonardo.upload.maximum_size=28672
leonardo.upload.maximum_data_size=2560
leonardo.upload.speed=57600
leonardo.upload.disable_flushing=true
leonardo.upload.use_1200bps_touch=true
leonardo.upload.wait_for_upload_port=true
 
leonardo.bootloader.tool=avrdude
leonardo.bootloader.low_fuses=0xff
leonardo.bootloader.high_fuses=0xd8
leonardo.bootloader.extended_fuses=0xcb
leonardo.bootloader.file=caterina/Caterina-Leonardo.hex
leonardo.bootloader.unlock_bits=0x3F
leonardo.bootloader.lock_bits=0x2F
 
leonardo.build.mcu=atmega32u4
leonardo.build.f_cpu=16000000L
leonardo.build.vid=0x2341
leonardo.build.pid=0x8036
leonardo.build.usb_product="Arduino Leonardo"
leonardo.build.board=AVR_LEONARDO
leonardo.build.core=arduino
leonardo.build.variant=leonardo
leonardo.build.extra_flags={build.usb_flags}
 
##############################################################
```

C:\Program Files (x86)\Arduino\hardware\arduino\avr\bootloaders\caterina\Makefile

bulid error issue

```
:~/source/Arduino/hardware/arduino/avr/bootloaders/caterina$ make
Makefile:153: ../../../../../../LUFA/LUFA-111009/LUFA/makefile: No such file or directory
make: *** No rule to make target '../../../../../../LUFA/LUFA-111009/LUFA/makefile'.  Stop.
 
```

[solution about the build error](http://forum.arduino.cc/index.php?topic=267125.0)

```
Try this download link, http://www.github.com/abcminiuser/lufa/archive/LUFA-111009.zip
 
I have my install paths like this:
\My Documents\Arduino\arduino-1.0.5-r2\hardware\arduino\bootloaders\caterina
\My Documents\LUFA\LUFA-111009\
```

[DEVICE\_VID build error](http://forum.arduino.cc/index.php?topic=267125.5;wap2) solution1, [solution2](http://forum.freetronics.com/viewtopic.php?t=663)

modify(uncomment) /Arduino/hardware/arduino/avr/bootloaders/caterina/Makefile

bootloader를 arduino board에 write 해야 한다.

[arduino ISP downloader](https://www.arduino.cc/en/Guide/ArduinoISP) - ISP downloader module을 사서 연결하는 방식 ([board](https://www.arduino.cc/en/Main/ArduinoISP))

[arduino ISP Downlaoder](https://www.arduino.cc/en/Tutorial/ArduinoISP) - arduino 두대를 연결해서 write 하는 방식

\