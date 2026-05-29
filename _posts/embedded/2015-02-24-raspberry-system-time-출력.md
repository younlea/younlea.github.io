---
title: "raspberry system time 출력"
excerpt_separator: "<!--more-->"
date: 2015-02-24 01:06:06 +0900
categories:
  - embedded
tags:
  - develop_env
  - embedded
  - 임베디드

toc : true
toc_sticky : true
---

app이 자꾸 종료가 되서 app 동작관련 로그를 심으려고 하는데 시간 정보가 필요해서 방법을 찾아봄

출처 : [time 확인](http://forum.falinux.com/zbxe/index.php?document_srl=408343&mid=C_LIB)

|  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- |
| |  |  | | --- | --- | | **설명** | 시스템의 시간을 구합니다. 구해지는 시간은 1970년 1월 1일 0시부터 함수를 호출할 때 까지의 초단위입니다. 그러므로 time()함수에서 구한 값으로는 지금이 몇 시인지 알기가 쉽지 않습니다. time()에서 구한 시간 정보를 알기 쉽게 문자열을 만들기 위해서는 ctime()함수를 이용하면 됩니다. | | **헤더** | time.h | | **형태** | **time\_t** time(**time\_t** \*t); | |
| |  |  |  | | --- | --- | --- | | **인수** | **time\_t** \*t | 시간정보를 받을 변수 | | **반환** | **time\_t** | 1970년 1월 1일 0시부터 함수를 호출할 때 까지의 초 카운트 | |
| time을 특정 파일에 저장하는 코드   ----------------- source code -------------------- #include <stdio.h> #include <time.h>  int  main(void) { FILE \*fp time\_t current\_time;  if(fp = fopen("./log.log", "a")) {  fputs("logtest....1 \n",fp);  fprintf(fp, "test \n"); time( &current\_time); fprintf(fp, ctime(&current\_time));  fclose(fp); } else  printf("cannot make log file\n");  return 0;  } ----------------- result ------------------- logtest....1 test Sun Dec  8 20:37:04 4461680 |  |  |  |