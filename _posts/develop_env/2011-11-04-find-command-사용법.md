---
title: "find command 사용법"
excerpt_separator: "<!--more-->"
date: 2011-11-04 13:24:10 +0900
categories:
  - develop_env
tags:
  - etc

toc : true
toc_sticky : true
---

기본적으로 현지 있는 directory에서 하위 폴더에 있는 파일을 찾을때

find -name usb\*.\*

[-name : file 이름으로 검색하다는걸 의미]

[usb\*.\* : usb로 시작하는 이름을 가진 file을 검색]

find -name usb -type d   : usb라는 이름을 가지고 있는 directory만을 보여준다.

[-type : 찾는 type]

[d       : directory type]

find -name makefile |xargs grep feature : makefile에서 feature 문자열을 찾아준다.

find -name \*.c |xargs grep -rni driver\*   : find driver string in c files.

-r : recursive

-n : line number

-i : ignore-case

아래와 같이 사용해도 됨.

find -name 파일이름 -exec grep -rni 찾을문자열 {} \;

\