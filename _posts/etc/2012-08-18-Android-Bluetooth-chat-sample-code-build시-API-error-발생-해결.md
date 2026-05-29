---
title: "Android Bluetooth chat sample code build시 API error 발생 해결"
excerpt_separator: "<!--more-->"
date: 2012-08-18 14:23:46 +0900
categories:
  - etc
tags:
  - etc

toc : true
toc_sticky : true
---

음.. 한번 빌드 되고 이후 부터

call requires API level5 (current min is 1):bluetooth.... 라는 에러가 나오면서 빌드가 안된다..

해결 책은....

프로젝트 에서 우클릭 -> Android Toos -> Clear Lint Markers

-> Fix Project Properties

이렇게 두번 클릭해 주고 빌드하면 해결된다..

![image](/assets/images/posts/5668568/c0028014_502f26a3583c7.jpg)