---
title: "Ubuntu file이 있는데 no such file or directory 라고 나올때..."
excerpt_separator: "<!--more-->"
date: 2011-11-09 16:11:22 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

문제의 원인은...

사용하고자 하는 file이 32bit로 만들어진 것었고.. OS는 64 bit로 되어 있어서.. 충돌이 난거다..

그런데 에러 메세지가.. no such file or directory로 나왔으니... ㅡㅜ

실행이 안되면.. 저런 에러가 나온다고 한다..

결국 64bit에서 32bit로 만들어진 실행파일을 실행하기 위해 아래 package를 깔면된다. ^^;

sudo apt-get install ia32-libs

아.. 행복해... (하루동안 별 삽질을 다했다 ㅡㅜ)