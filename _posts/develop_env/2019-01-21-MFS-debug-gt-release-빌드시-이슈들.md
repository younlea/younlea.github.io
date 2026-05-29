---
title: "MFS debug -&gt; release 빌드시 이슈들"
excerpt_separator: "<!--more-->"
date: 2019-01-21 15:13:55 +0900
categories:
  - develop_env
tags:
  - etc

toc : true
toc_sticky : true
---

1.  이상하게 cstring이 동작 안한다.

project property -> Confinguration Properties -> General -> Project Defaults.

Characher Set : USE Multi-Byte Character Set

2. release 모드로 빌드시 라이브러리 넣어서 빌드해야 할 경우

project property -> Confinguration Properties -> General -> Project Defaults.

USE of MFC : Use MFC in a Static Library

project property -> C/C++ -> Code Generation

Runtime Library : Multi-threaded(/MT)

\