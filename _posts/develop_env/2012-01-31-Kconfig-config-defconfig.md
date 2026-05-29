---
title: "Kconfig, .config defconfig"
excerpt_separator: "<!--more-->"
date: 2012-01-31 15:54:35 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

Linux Kernel빌드를 하다보면.. 신기한게 있는데..

make menuconfig 이다.. 요거.. 하면 UI가 나오면서 build option들을 선택하게 된다.

그리고 .config에 저장이 된다.

그러면 이걸 그대로 빌드하면 적용되냐? 그건 아니더군..

arch/arm/configs/defconfig에 저장이되어 있어야 한단다.

.config는 두가지 경우에 생성이 되는데.

1. make menuconfig.

2. build

이때 defconfig와 Kconfig들의 조합으로 생성이 된다고 보면 된다.

빌드를 하고 만들어지는 file은 autoconf.h로..

.include/generated/autoconf.h 에 위치한다.

두서 업이 적어 놓기는 했는데..

결론은.. .config는 확인용으로만 쓰인다는거...

build는 defconfig를 수정해야한다는거 정도 알면될듯 하다.