---
title: "shell 무한 반복 script"
excerpt_separator: "<!--more-->"
date: 2015-02-25 00:11:09 +0900
categories:
  - etc
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

bash shell로 특정 APP을 지속적으로 살려야 하는 경우가 생겼다.

관련 기능을 구현하기 위해 아래와 같이 app을 지속적으로 실행하는 script를 만들어서 해결하였다.

test   <<-- bash shell file (test.exe를 무한 반복)

------------------------------------

#!/bin/bash

while :

do

echo "test~~~~"

./test.exe

sleep 2

done

------------------------------------

test.exe  <<--실행될 exe file

------------------------------------

#!/bin/bash

echo "test.exe~~~~"

sleep 2

------------------------------------

test는 $./test로 해 보면 된다. (아 shell이 실행 가능하도록 chmod 777 해야함 ^^; )

특정 케이스에 owner를 바꿔야 하는데..

chown developer:developer test   << 이렇게 하면 test의  owner가 developer develpoer 로 바뀐다.