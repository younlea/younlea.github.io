---
title: "Toast message"
excerpt_separator: "<!--more-->"
date: 2012-04-05 12:09:46 +0900
categories:
  - develop_env
tags:
  - etc

toc : true
toc_sticky : true
---

Toast.makeText(this, "test !!!!", Toast.LENGTH\_SHORT).show();

간혹 listener안에서 this가 안 먹을때가 있는데 이때는 아래처럼 activity에 this를 선언해서 처리한다.

Toast.makeText(FullScreenTextActivity.this, "test !!!!", Toast.LENGTH\_SHORT).show();