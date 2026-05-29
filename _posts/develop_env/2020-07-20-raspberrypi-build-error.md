---
title: "raspberrypi build error."
excerpt_separator: "<!--more-->"
date: 2020-07-20 04:53:58 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

package 받아서 빌드하는데 이런 에러가 나오면 당혹스럽다 ㅡ.ㅡ;

/lib/modules/4.19.118-v7+/build: No such file or directory.  Stop

확인해 보면.. /lib/modules/...version.../build라는 폴더가 없어서 인데. .이것

ln으로 /usr/src/ 밑에 빌드용 헤더가 없어서 나오는 문제다 .ㅡㅡ;

찾아보면.. 아래 링크에 대처법들이 있는데..

<https://www.raspberrypi.org/forums/viewtopic.php?t=67347>

정리하면.. ㅡ.ㅡ 모 강제로 받아서 링크 넣는 방법과 패키지를 받는 방법 두가지가 있다 ㅡ.ㅡ;

일단 첫번째껄루 해보려다 삽질해서 그냥 두번째 껄루 해보려고 한다.

그런데. 정식 버젼이면.. 아래 명령 하나만 해줘도 된다. ㅡ.ㅡ

sudo apt-get install linux-headers

\