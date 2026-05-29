---
title: "avrdude - leonardo download 방법"
excerpt_separator: "<!--more-->"
date: 2016-01-12 02:03:12 +0900
categories:
  - embedded
tags:
  - develop_env
  - embedded
  - 임베디드

toc : true
toc_sticky : true
---

UNO를 IDE통하지 않고 avrdude를 통해서 받는 방법은

avrdude -v -c arduino -p m328p -P com18 -U flash:w:Blink\_uno.hex:i -C./avrdude\_ori.conf

```
avrdude: Version 6.0.1, compiled on Apr 15 2015 at 19:59:58
         Copyright (c) 2000-2005 Brian Dean, http://www.bdmicro.com/
         Copyright (c) 2007-2009 Joerg Wunsch
 
         System wide configuration file is "./avrdude_ori.conf"
 
         Using Port                    : com18
         Using Programmer              : arduino
         AVR Part                      : ATmega328P
         Chip Erase delay              : 9000 us
         PAGEL                         : PD7
         BS2                           : PC2
         RESET disposition             : dedicated
         RETRY pulse                   : SCK
         serial program mode           : yes
         parallel program mode         : yes
         Timeout                       : 200
         StabDelay                     : 100
         CmdexeDelay                   : 25
         SyncLoops                     : 32
         ByteDelay                     : 0
         PollIndex                     : 3
         PollValue                     : 0x53
         Memory Detail                 :
 
                                  Block Poll               Page                       Polled
           Memory Type Mode Delay Size  Indx Paged  Size   Size #Pages MinW  MaxW   ReadBack
           ----------- ---- ----- ----- ---- ------ ------ ---- ------ ----- ----- ---------
           eeprom        65    20     4    0 no       1024    4      0  3600  3600 0xff 0xff
           flash         65     6   128    0 yes     32768  128    256  4500  4500 0xff 0xff
           lfuse          0     0     0    0 no          1    0      0  4500  4500 0x00 0x00
           hfuse          0     0     0    0 no          1    0      0  4500  4500 0x00 0x00
           efuse          0     0     0    0 no          1    0      0  4500  4500 0x00 0x00
           lock           0     0     0    0 no          1    0      0  4500  4500 0x00 0x00
           calibration    0     0     0    0 no          1    0      0     0     0 0x00 0x00
           signature      0     0     0    0 no          3    0      0     0     0 0x00 0x00
 
         Programmer Type : Arduino
         Description     : Arduino
         Hardware Version: 3
         Firmware Version: 4.4
         Vtarget         : 0.3 V
         Varef           : 0.3 V
         Oscillator      : 28.800 kHz
         SCK period      : 3.3 us
 
avrdude: AVR device initialized and ready to accept instructions
 
Reading | ################################################## | 100% 0.01s
 
avrdude: Device signature = 0x1e950f
avrdude: safemode: lfuse reads as 0
avrdude: safemode: hfuse reads as 0
avrdude: safemode: efuse reads as 0
avrdude: NOTE: "flash" memory has been specified, an erase cycle will be performed
         To disable this feature, specify the -D option.
avrdude: erasing chip
avrdude: reading input file "Blink_uno.hex"
avrdude: writing flash (1030 bytes):
 
Writing | ################################################## | 100% 0.36s
 
avrdude: 1030 bytes of flash written
avrdude: verifying flash memory against Blink_uno.hex:
avrdude: load data flash data from input file Blink_uno.hex:
avrdude: input file Blink_uno.hex contains 1030 bytes
avrdude: reading on-chip flash data:
 
Reading | ################################################## | 100% 0.33s
 
avrdude: verifying ...
avrdude: 1030 bytes of flash verified
 
avrdude: safemode: lfuse reads as 0
avrdude: safemode: hfuse reads as 0
avrdude: safemode: efuse reads as 0
avrdude: safemode: Fuses OK (H:00, E:00, L:00)
 
avrdude done.  Thank you.
```

그러나 레오나르도는 안된다..

해결책은 아래 주소에서 찾았다.

https://nicholaskell.wordpress.com/tag/leonardo/

요는..

1. 다운로드 모드로 들어가게 한다. (아래 명령을 써도 되고 아두이노 씨리얼에서 그냥 열었다 닫으면 된다.)

avrdude -v -c arduino -p m32u4 -P com20 -C./avrdude\_ori.conf **-b** 1200

2. 다운로드를 한다.

avrdude -v -c avr109 -p m32u4 -P COM15 -U flash:w:co.hex:i -C./avrdude\_ori.conf