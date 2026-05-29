---
title: "grep macro 만들기.."
excerpt_separator: "<!--more-->"
date: 2021-02-04 19:37:16 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

흠.. grep을 쓰다보면.. --include 나 $(find -iname xx.x) 이런식으로 검색하는걸 줄이는데. 이게 타이핑할때마다 귀찮아서

간단한 매크로를 만들었다.

grepm.sh

```
  #!/bin/bash
  grep -rnw --include=*.c --include=*.h --include=*.cpp --include=*.mk --color $1 .
 
```

사용법

grepm.sh XXX

잘 나온다 흐흐흐

혹시 찾을때 더 필요한 파일이 있으면 저기  include를 추가해 주면 됩니다요.

version2

```
#!/bin/bash
grep -rnw --include=*.c --include=*.h --include=*.cpp --include=*.mk --include=Makefile --include=*.ld --include=*.s --include=*.S --include=*.map --color $1 .
 
```

\