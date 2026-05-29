---
title: "shell script 만드는 간단 tip"
excerpt_separator: "<!--more-->"
date: 2012-07-06 11:08:29 +0900
categories:
  - develop_env
tags:
  - etc

toc : true
toc_sticky : true
---

아래처럼 xxx.sh 를 만든다.

ex)

#!/bin/sh

mount -t nfs -o nolock xxx.xxx.xxx.xxx:/home/xxxx/nfs   /mnt/nfs

이후... 실행하면.. 안된다 ㅡ..ㅡ 왜???

ls -al로 쳐보면 알겠지만. r 권한만 있다..

이때 두가지 실행 방법이 있다.

#ss xxx.sh 로 실행

아니면.. mode를 바꾸는 방법이 있다.. 이게 더 편함

#chmod -x xxx.sh

이렇게 하고 ./xxx.sh 하면 된다.. 이야 편하다..