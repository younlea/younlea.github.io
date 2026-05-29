---
title: "ARTIK uboot make menuconfig 안될때.."
excerpt_separator: "<!--more-->"
date: 2017-06-21 17:35:43 +0900
categories:
  - embedded
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

xxx@sulac:~/SourceCode/ignis/ignis/software/u-boot-artik$ make menuconfig

HOSTCC  scripts/kconfig/mconf.o

**In file included from scripts/kconfig/mconf.c:23:0:**

scripts/kconfig/lxdialog/dialog.h:38:20: fatal error: curses.h: No such file or directory

compilation terminated.

scripts/Makefile.host:108: recipe for target 'scripts/kconfig/mconf.o' failed

make[1]: \*\*\* [scripts/kconfig/mconf.o] Error 1

Makefile:477: recipe for target 'menuconfig' failed

make: \*\*\* [menuconfig] Error 2

관련 에러는 ncurses package가 깔려 있지 않아서 이다.

해결책은 아래 패키치 인스톨 하면 된다.

sudo apt-get install libncurses5-dev

혹시 빌드를 위해서는 아래 방법도 있는데 하다가 포기함.

https://geeksww.com/tutorials/operating\_systems/linux/tools/how\_to\_download\_compile\_and\_install\_gnu\_ncurses\_on\_debianubuntu\_linux.php