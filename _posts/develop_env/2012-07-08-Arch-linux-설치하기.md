---
title: "Arch linux 설치하기."
excerpt_separator: "<!--more-->"
date: 2012-07-08 23:10:48 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

모..

CD만들어서 넣고 나서..

부팅하면.. 아래 처럼 치라고 나온다..

#/arch/setup

이후 과정은 대충 셋팅해서 까는데.. 우선 정리는 못했지만. 한글로 된 가이드가 있어서 링크를 건다..

<https://wiki.archlinux.org/index.php/Beginners%27_Guide_(%ED%95%9C%EA%B5%AD%EC%96%B4)>

움. 우선 네트워크 셋팅이 최초에 해야할것 같다..

dhcpcd eth0 를 하면 자동으로 잡아준다. 으헤헤

그리고 ping -c 3 www.google.com 으로 동작이 되면.. 연결된거다. ^^;

system upgrade는 pacman을 이용해서 한다. (ubuntu에서는 apt-get을 사용했었는데.. 새로운 거네...)

<http://wiki.kldp.org/wiki.php/ArchInstall>

http://arch.korea.com/

http://arch.korea.com/viewtopic.php?id=22

Xwindows 설치하기.

pacman repository setting.. (아래 파일에서 korea를 찾고 풀어줌.)

vi /etc/pacman.d/mirrorlist

pacman이 잘 안된다 ...

could not open file /var/lib/pacman/sync/core.db ... 이런 에러가 나는데...

**pacman -Syuf** 요렇게 하면 sync 맞추는것 같다.

X설치할라는데... 또 에러가 발생한다..

```
# pacman -S xorg-server xorg-xinit xorg-server-utils
```

error: failed to commit transaction (conflicting files)....

\