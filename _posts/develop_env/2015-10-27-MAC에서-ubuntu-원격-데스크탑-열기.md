---
title: "MAC에서  ubuntu 원격 데스크탑 열기"
excerpt_separator: "<!--more-->"
date: 2015-10-27 00:40:28 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

아래와 같이 ssh를 열어서 X를 실행하면 된다.

```
$ ssh -X root@192.168.0.4
$ startkde
```