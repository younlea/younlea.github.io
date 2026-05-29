---
title: "grep command 사용법."
excerpt_separator: "<!--more-->"
date: 2011-11-04 13:29:24 +0900
categories:
  - develop_env
tags:
  - etc

toc : true
toc_sticky : true
---

[20111104]

grep의 경우 file 안에 있는 코드들을 검색시 사용한다.

grep 명령을 치면 grep [OPTION]... PATTERN [FILE]...

Try 'grep --help' for more information

이라고 나온다 ^^

결국 grep option pattern file종료로 생각하면된다.

예로는

grep -r usb \*  : usb라는 pattern을 모든 종류의 파일에 대해서 하위 디렉토리 모두를 검색하라

[-r]하위 디렉토리까지 검색

만약 나오는 내용이 많다면 마지막에 [ | more]를 추가해서 명령하면 page 단위로 나오게 된다.

ls -al | grep u-boot

ls형석으로 u-boot가 있는 파일들을 찾아라.

검색

grep option patten start\_directory.

what을 검색해 보자....

grep -rni what ./

현재 디렉토리 아래로 what이라는 패턴을 대소문자 구분없이 라인까지 넣어서 보여준다.

특정 파일종류에서 찾고 싶다.

[--include=\*.c]

grep -rni --include=\*c waht ./

--include=\*config/-.c

[]를 사용해서 c나 header만 확인도 가능

--include=\*.[ch]

여러 특정 파일 조건에서도 찾기가 가능하네..

grep -rni --include=\*.c --include=\*tar\*.mak what ./

검색된 라인의 앞뒤가 궁굼하다.?

-C 5 앞뒤로 5줄

-A 5 뒤에 5줄

-B 5 앞에 5줄

grep -rni -C 5 --include=\*.c what ./