---
title: "FILE function 관련"
excerpt_separator: "<!--more-->"
date: 2011-07-02 10:28:04 +0900
categories:
  - develop_env
tags:
  - etc

toc : true
toc_sticky : true
---

fopen시 error값 확인 하기..

FILE \*fp;

fp = fopen("temp.bin","w");  //<<   ..\.. 아니고... ..\\..\\.. 로 써야 한다.

if(fp == NULL)

printf("can't open printer; ERROR: %d %s\n", errno, strerror(errno));

//위와 같이 하면 error 값을 return 할수 있다. ^^;

file size check

int f\_size =0;

fseek(fp, 0, SEEK\_END);   // fp 파일의 마지막으로 간다.

f\_size = ftell(fp);  // 현재 열린 파일의 마지막 위치를 알려준다. 결국 요게 size가 되죠.. ^^

rewind(fp);

file open할때. 옵션이 있는데..

r 읽기 위해

w 쓰기 위해

a 없으면 만들고 있으면 뒤에 붙혀서 쓰기위해..

+b : binary type... 주로 바이너리는 이걸로 해야하는데 안붙이면 이상하게 size가 뻥튀기 된다. ㅡㅜ

\