---
title: "malloc function call로 처리하기?"
excerpt_separator: "<!--more-->"
date: 2011-03-16 13:57:38 +0900
categories:
  - algorithm
tags:
  - algorithm
  - 알고리즘

toc : true
toc_sticky : true
---

보통.. heap에 올릴때 malloc을 자주 사용한다..

int 형일때는 간단히 아래처럼 사용하고 없애는데...

int \* buffer;

if((buffer = (int \*)malloc(sizeof(int)\*MAX\_BUFF) == NULL)

printf("메모리 부족 ㅡ.ㅡ;\n");

for(int i =0; i < MAX\_BUFF; I++)

buffer[i]=i;

free(buffer);

그런데 함수호출해서 만들때는 어떻게 해야할까??

아래처럼 하면되네.. 흠냐.

#include <stdio.h>

#include <stdlib.h>

#define MAX\_TX\_BUFFER\_SIZE 100

void create\_tx\_buffer( int \*\* buffer)

{

\*buffer = (int\*)malloc(sizeof(int)\*MAX\_TX\_BUFFER\_SIZE);

}

void finish\_tx\_buffer(int \*buffer)

{

free(buffer);

printf("free(buffer)\n");

}

void using\_buffer(int \* buffer)

{

int i = 0;

for(i =0; i < MAX\_TX\_BUFFER\_SIZE; i++)

buffer[i]=i;

for(i =0; i < MAX\_TX\_BUFFER\_SIZE; i++)

printf("%d \n",\*(buffer+i));

}

void main(void)

{

int \* buffer;

create\_tx\_buffer(&buffer);

using\_buffer(buffer);

finish\_tx\_buffer(buffer);

}

모 그냥 아래처럼 써도 된다..

#include <stdio.h>

#include <stdlib.h>

#define MAX\_TX\_BUFFER\_SIZE 100

int \*buffer\_addr;

void create\_tx\_buffer()

{

buffer\_addr = (int\*)malloc(sizeof(int)\*MAX\_TX\_BUFFER\_SIZE);

}

void remove\_tx\_buffer(void)

{

free(buffer\_addr);

buffer\_addr = NULL;

printf("free(buffer)\n");

}

void using\_buffer(void)

{

int i = 0;

for(i =0; i < MAX\_TX\_BUFFER\_SIZE; i++)

buffer\_addr[i]=i;

for(i =0; i < MAX\_TX\_BUFFER\_SIZE; i++)

printf("%d \n",\*(buffer\_addr+i));

}

void main(void)

{

create\_tx\_buffer();

using\_buffer();

remove\_tx\_buffer();

}