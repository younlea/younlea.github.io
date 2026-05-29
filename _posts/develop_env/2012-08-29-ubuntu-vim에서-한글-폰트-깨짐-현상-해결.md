---
title: "ubuntu vim에서 한글 폰트 깨짐 현상 해결"
excerpt_separator: "<!--more-->"
date: 2012-08-29 15:22:51 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

.vimrc에 아래 코드 추가

set fencs=utf-8,euc-kr,cp949,cp932,euc-jp,shift-jis,big5,latin1,ucs-2le

set fileencoding=cp949