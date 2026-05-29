---
title: "#if !defined() 용법에 대해서 (신기하잖아~~ 처음봐~)"
excerpt_separator: "<!--more-->"
date: 2005-08-04 08:13:32 +0900
categories:
  - etc
tags:
  - etc

toc : true
toc_sticky : true
---

#include \
#ifdef \_\_\_\_\_\_\_\_\_\_what\_\_\_\_\_\_\_\_\_\_\_\_\_

=============================\
#if !defined(FEATURE\_SAMSUNG) 사용 용법..\
=============================

선언되어 있지 않으면이라는 용법이다. \
처음 보는 방법이니 만큼 기억하길 바란다. 

#endif

#define FEATURE\_SAMSUNG

int main(int argc, char \*argv[])\
{\
#if !defined(FEATURE\_SAMSUNG)\
printf("Hello, world\
");\
#endif\
return 0;\
}\
\