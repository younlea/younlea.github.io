---
title: "#ifndef 관련 (header file 관련 조사중)"
excerpt_separator: "<!--more-->"
date: 2005-08-09 11:41:41 +0900
categories:
  - dataScience
tags:
  - dataScience
  - datascience
  - 데이터

toc : true
toc_sticky : true
---

#include \
#ifdef \_\_\_\_\_what\_\_\_\_\_\_\_\
 이번에는 헤더파일을 분석하다가나온내용을 테스트 해보았다.\
 #ifndef \_STDIO\_H\_\
 #define \_STDIO\_H\_\
 위와 같이 선언되어 있었는데 그 이유는 결국 여러번 include 될때\
 한번만 실행되게하기 위해서라고 한다. 테스트 결과도 맞는것 같다.

 참고로 #ifndef 라는것이다. #ifdef 가아니라.. \
 == if not define\
 \
#endif\
int main(int argc, char \*argv[])\
{\
#ifndef \_test\_\
#define \_test\_\
printf("과연 성공할까??\
");\
#endif //\_test\_\
printf("것봐 안되잖아");\
return 0;\
}\
\