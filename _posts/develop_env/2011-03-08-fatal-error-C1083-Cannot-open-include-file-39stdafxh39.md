---
title: "fatal error C1083: Cannot open include file: &#39;stdafx.h&#39;:"
excerpt_separator: "<!--more-->"
date: 2011-03-08 18:56:39 +0900
categories:
  - develop_env
tags:
  - etc

toc : true
toc_sticky : true
---

음냐.. 아무리 봐도 stdafx.h는 있는데 자꾸 에러가 날때......

이리저리 찾다가.. 보니...

두가지 방법이 있더군요.

1. debug와 release 폴더를 통채로 지우고 다시 빌드... 이렇게 하면 거의 되긴 합니다.

2. alt+f7 -> C/C++ -> Category(precompiled headers) -> Not using precompiled headers. 로 해보고

안되면 다시 Automatic use of precompiled header로 바꿔보면 잘 됩니다. ^^;

\