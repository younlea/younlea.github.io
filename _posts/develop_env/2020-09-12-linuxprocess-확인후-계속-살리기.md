---
title: "[linux]process 확인후 계속 살리기"
excerpt_separator: "<!--more-->"
date: 2020-09-12 19:47:41 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

#!/bin/bash

while [ 1 ]

do

Cnt=`ps -aux|grep "seekware-tcpip"|grep -v grep|wc -l`

PROCESS=`ps -aux|grep "seekware-tcpip"|grep -v grep|awk '{print $1}'`

if [ $Cnt -ne 0 ]

then

echo "seekware-tcpip (PID : $PROCESS) alredy runing"

else

seekware-tcpip &

echo "seekware-tcpip run"

fi

sleep 3

done

\