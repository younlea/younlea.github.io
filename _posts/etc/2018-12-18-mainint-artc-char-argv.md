---
title: "main(int artc, char *argv[])"
excerpt_separator: "<!--more-->"
date: 2018-12-18 00:32:04 +0900
categories:
  - etc
tags:
  - dataScience
  - datascience
  - 데이터

toc : true
toc_sticky : true
---

움 기억이 가물가물해서.. 정리 차원에서 적어 둡니다.

코딩을 하고 실행파일 뒤로 입력을 받을때

./a.out input1 input2 input3 이렇게 넣게 되는데 이걸 받는걸 자꾸 까먹어서 정리를.. ^^;

일단 아래처럼 c, v로 선언했는데 대부분 일반적으로 argc, \*argv를 쓰기는 하는데 prefix인지 확인차원에서 해보니 그냥 아무 이름이나 써도 되네요.

아래 코멘트 처럼 입력된 값들의 갯수와 그 갯수별 데이터 내용입니다.

#include <stdio.h>

int main(int c, char \*v[]) // c : 실행문 다음에 오는 입력값 카운트, \*v[] 입력된 스트링 값.

{

printf("%d \n",c);

for(int i = 0; i < c; )

{

printf("argc[%d] argv[%d]:[%s]\n", i, i, v[i]);

i++;

}

return 0;

}