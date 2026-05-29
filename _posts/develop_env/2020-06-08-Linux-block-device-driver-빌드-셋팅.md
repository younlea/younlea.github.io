---
title: "Linux block device driver 빌드 셋팅"
excerpt_separator: "<!--more-->"
date: 2020-06-08 16:46:57 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

리눅스 block device driver를 만들려고 하는데.. sample code를 찾아도 무언가 부족하다.

뭐가 부족하냐면 빌드 할때 header file을 못찾는 경우가 많아서다.. ㅜㅜ

빌드를 할때 compiler와 header file들의 위치를 알아야 하는데... 이게 정리가 잘 안되어 있어서 살짝 정리해 보려고 한다.

ubuntu board 에 해당 kernel version이 있는곳

/lib/module/4.15.0-46-generic/build  <<< 여기서 4.15.0-46-generic 는 uname -r 로 확인하면 알수 있다.

그런데.. 우리가 보통 타겟 보드에서 쓸 모듈을 데스크탑에서 빌드를 해서 넣게 된다.

그러다 보니 cross compiler와 해당 보드에 맞는 linux hearder file이 있어야 한다. ㅜㅜ

해당 보드에 맞는 linux header file을 받는 법은 github에서 linux를 찾아서 버젼을 씽크하는 방법도 있고..

아래 방법으로 설치가 가능하다.

$uname -r 로 버젼을 확인후

$ls -l /usr/src/linux-headers-$(uname -r) 로 했을때 없으면..

$sudo apt update

$sudo apt install linux-headers-$(uname -r)로 설치를 한다.

github로 받는것 보다 요게 더 편한듯 하다. ^^;

자 그럼 이제 build를 하는 환경 셋팅이 필요한데 보통 Makefile에서 작업을 하게 된다.

참 arm cross compiler는 아래와 같이 설치 하면 된다.

$sudo add-apt-repository ppa:linaro-maintainers/toolchain

$sudo apt-get update

$sudo apt-get install gcc-arm-linux-gnueabi

설치 버젼 확인은

$arm-linux-gnueabi-gcc  --veresion

자.. 그럼 이제 ... 아래 준비물들이 준비가 되었을겁니다.

1. sample source code << 이건 알아서 ㅋㅋ

2. arm-gcc

3. linux header file.

그럼 빌드를 할때 세개가 잘 어우려 져야 하잖아요. 요건 Make file에서 하게 됩니다.

Makefiel sample

CC :=/usr/.....   << arm gcc가 설치된 위치.

obj-m := hello.o

KDIR :=/lib/module/$(shell uname -r)/build << linux kernel header가 있는곳 (Kernel source code가 있는 최상위)

all :

make -C $(KDIR) SUBDIRS=$(PWD) modules

clean:

rm -rf \*.o \*.ko \*.mod.\* \*.cmd

\