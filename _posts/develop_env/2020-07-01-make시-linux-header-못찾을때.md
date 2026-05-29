---
title: "make시 linux header 못찾을때"
excerpt_separator: "<!--more-->"
date: 2020-07-01 05:43:16 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

빌드를 하려는데 linux header파일을 아래와 같이 못 찾을때가 있다.

pi@raspberrypi:~/source/thermal/src/device-driver $ make

make -C /lib/modules/5.4.35-v7+/build SUBDIRS=/home/pi/source/thermal/src/device-driver modules

make[1]: \*\*\* /lib/modules/5.4.35-v7+/build: No such file or directory.  Stop.

Makefile:5: recipe for target 'default' failed

make: \*\*\* [default] Error 2

보통 현재 설치된 버젼에 맞춰서 아래와 같이 명령을 하면 설치가 되어야 한다. 

pi@raspberrypi:~/source/thermal/src/device-driver $ sudo apt install linux-headers-$(uname -r)

Reading package lists... Done

Building dependency tree       

Reading state information... Done

E: Unable to locate package linux-headers-5.4.35-v7

E: Couldn't find any package by glob 'linux-headers-5.4.35-v7'

E: Couldn't find any package by regex 'linux-headers-5.4.35-v7'

흑 그런데 안된다. 일반적인거라도 설치해 볼까?

pi@raspberrypi:~/source/thermal/src/device-driver $ sudo apt-get install linux-generic

Reading package lists... Done

Building dependency tree       

Reading state information... Done

E: Unable to locate package linux-generic

안된다 ㅜㅜ 

다운가능한게 뭐가 있는데?  확인해 보자

$apt-cache search linux-headers

linux-headers-3.10-3-all - All header files for Linux 3.10 (meta-package)

linux-headers-3.10-3-all-armhf - All header files for Linux 3.10 (meta-package)

linux-headers-3.10-3-common - Common header files for Linux 3.10-3

linux-headers-3.10-3-rpi - Header files for Linux 3.10-3-rpi

linux-headers-3.16.0-4-all - All header files for Linux 3.16 (meta-package)

linux-headers-3.16.0-4-all-armhf - All header files for Linux 3.16 (meta-package)

linux-headers-3.16.0-4-common - Common header files for Linux 3.16.0-4

linux-headers-3.16.0-4-rpi - Header files for Linux 3.16.0-4-rpi

linux-headers-3.18.0-trunk-all - All header files for Linux 3.18 (meta-package)

linux-headers-3.18.0-trunk-all-armhf - All header files for Linux 3.18 (meta-package)

linux-headers-3.18.0-trunk-common - Common header files for Linux 3.18.0-trunk

linux-headers-3.18.0-trunk-rpi - Header files for Linux 3.18.0-trunk-rpi

linux-headers-3.18.0-trunk-rpi2 - Header files for Linux 3.18.0-trunk-rpi2

linux-headers-3.6-trunk-all - All header files for Linux 3.6 (meta-package)

linux-headers-3.6-trunk-all-armhf - All header files for Linux 3.6 (meta-package)

linux-headers-3.6-trunk-common - Common header files for Linux 3.6-trunk

linux-headers-3.6-trunk-rpi - Header files for Linux 3.6-trunk-rpi

linux-headers-4.4.0-1-all - All header files for Linux 4.4 (meta-package)

linux-headers-4.4.0-1-all-armhf - All header files for Linux 4.4 (meta-package)

linux-headers-4.4.0-1-common - Common header files for Linux 4.4.0-1

linux-headers-4.4.0-1-rpi - Header files for Linux 4.4.0-1-rpi

linux-headers-4.4.0-1-rpi2 - Header files for Linux 4.4.0-1-rpi2

linux-headers-4.9.0-6-all - All header files for Linux 4.9 (meta-package)

linux-headers-4.9.0-6-all-armhf - All header files for Linux 4.9 (meta-package)

linux-headers-4.9.0-6-common - Common header files for Linux 4.9.0-6

linux-headers-4.9.0-6-common-rt - Common header files for Linux 4.9.0-6-rt

linux-headers-4.9.0-6-rpi - Header files for Linux 4.9.0-6-rpi

linux-headers-4.9.0-6-rpi2 - Header files for Linux 4.9.0-6-rpi2

linux-headers-rpi - Header files for Linux rpi configuration (meta-package)

linux-headers-rpi-rpfv - This metapackage will pull in the headers for the raspbian kernel for the

linux-headers-rpi2 - Header files for Linux rpi2 configuration (meta-package)

linux-headers-rpi2-rpfv - This metapackage will pull in the headers for the raspbian kernel for the

linux-libc-dev-alpha-cross - Linux Kernel Headers for development (for cross-compiling)

linux-libc-dev-arm64-cross - Linux Kernel Headers for development (for cross-compiling)

linux-libc-dev-armel-cross - Linux Kernel Headers for development (for cross-compiling)

linux-libc-dev-armhf-cross - Linux Kernel Headers for development (for cross-compiling)

linux-libc-dev-hppa-cross - Linux Kernel Headers for development (for cross-compiling)

linux-libc-dev-m68k-cross - Linux Kernel Headers for development (for cross-compiling)

linux-libc-dev-mips-cross - Linux Kernel Headers for development (for cross-compiling)

linux-libc-dev-mips64-cross - Linux Kernel Headers for development (for cross-compiling)

linux-libc-dev-mips64el-cross - Linux Kernel Headers for development (for cross-compiling)

linux-libc-dev-mipsel-cross - Linux Kernel Headers for development (for cross-compiling)

linux-libc-dev-powerpc-cross - Linux Kernel Headers for development (for cross-compiling)

linux-libc-dev-powerpcspe-cross - Linux Kernel Headers for development (for cross-compiling)

linux-libc-dev-ppc64-cross - Linux Kernel Headers for development (for cross-compiling)

linux-libc-dev-ppc64el-cross - Linux Kernel Headers for development (for cross-compiling)

linux-libc-dev-s390x-cross - Linux Kernel Headers for development (for cross-compiling)

linux-libc-dev-sh4-cross - Linux Kernel Headers for development (for cross-compiling)

linux-libc-dev-sparc64-cross - Linux Kernel Headers for development (for cross-compiling)

raspberrypi-kernel-headers - Header files for the Raspberry Pi Linux kernel

많네.. ^^; 여기서 하나 받아 보자.

pi@raspberrypi:~/source/thermal/src/device-driver $ sudo apt install raspberrypi-kernel-headers

Reading package lists... Done

Building dependency tree       

Reading state information... Done

The following package was automatically installed and is no longer required:

  realpath

Use 'sudo apt autoremove' to remove it.

The following NEW packages will be installed:

  raspberrypi-kernel-headers

0 upgraded, 1 newly installed, 0 to remove and 154 not upgraded.

Need to get 16.7 MB of archives.

After this operation, 109 MB of additional disk space will be used.

Get:1 http://archive.raspberrypi.org/debian stretch/main armhf raspberrypi-kernel-headers armhf 1.20190819~stretch-1 [16.7 MB]

Fetched 16.7 MB in 7s (2,341 kB/s)                                                                                                                                                                                                           

Selecting previously unselected package raspberrypi-kernel-headers.

(Reading database ... 145087 files and directories currently installed.)

Preparing to unpack .../raspberrypi-kernel-headers\_1.20190819~stretch-1\_armhf.deb ...

Unpacking raspberrypi-kernel-headers (1.20190819~stretch-1) ...

Progress: [ 16%] [####################################................................

엄.. 그런데 안된다. 그냥 /lib/modules를 보니까 몇개 있는데??? 그걸로 셋팅하다.

make file 수정.. ~~~

obj-m :=thermal-device-driver.o 

KDIR :=/lib/modules/4.19.66-v7+/build  <<< 여기를 바꾸면 된다. ^^;                                                       

PWD := $(shell pwd)

default:                                                                                                                     $(MAKE) -C $(KDIR) SUBDIRS=$(PWD) modules

clean:                                                                                                                  rm -rf \*.order \*.symvers \*.ko \*.mod.\* \*.o   

빌드 잘된다 ^^;

\