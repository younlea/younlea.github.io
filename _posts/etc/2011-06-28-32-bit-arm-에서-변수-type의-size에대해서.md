---
title: "32 bit arm 에서 변수 type의 size에대해서"
excerpt_separator: "<!--more-->"
date: 2011-06-28 22:09:26 +0900
categories:
  - etc
tags:
  - etc

toc : true
toc_sticky : true
---

기초적인건데 몰라서 붛여 넣습니다. 하..

sizeof(char) => 1

sizeof(int) => 32 이니다. 모 unsigned도 똑같죠.

그러니까.. 0xff를 <<16까지 해도.. int는 넘치지 않겠죠.. 아웅..

0xff<<16 ------ 0xff0000 이거니까요..  기본기에 충실합시다..

![image](/assets/images/posts/5510366/c0028014_4e09d20e144f8.jpg)