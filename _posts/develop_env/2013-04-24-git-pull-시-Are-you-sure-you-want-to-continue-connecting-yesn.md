---
title: "git pull 시 Are you sure you want to continue connecting (yes/no)? 항상 yes로 하고 싶을때"
excerpt_separator: "<!--more-->"
date: 2013-04-24 08:35:31 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

git pull을 자동으로 하고 싶은데... 아래와 같이 yes를 쳐야 해서 자동 스크립트를 못만들때가 있다.

> Are you sure you want to continue connecting (yes/no)?

요건 찾아보니.. 아래와 같이 스크립트를 ssh에 추가해주면 된다. 아래 빨간색은 본인이 사용하는 git host name을  써야함. ^^;

> $ echo -e "Host github.com\n\tStrictHostKeyChecking no\n" >> ~/.ssh/config

\