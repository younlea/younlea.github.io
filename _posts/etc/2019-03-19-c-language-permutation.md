---
title: "c language - permutation."
excerpt_separator: "<!--more-->"
date: 2019-03-19 08:26:15 +0900
categories:
  - etc
tags:
  - etc

toc : true
toc_sticky : true
---

훔. 자꾸 삽질해서. 정리..

개인 코드.. 대충짜서 그런지 ㅡ.ㅡ; 다시 수정해야 할듯. ^^;

결국 요는... my\_per에 있는 for문에서 잘 정리해야 한다는거..

int buff[3];

void my\_swap(int \* a, int \* b)

{

int \* tmp = NULL;

tmp = (int\*)malloc(sizeof(int));

\*tmp = \*a;

\*a = \*b;

\*b = \*tmp;

free(tmp);

}

void my\_per(int start, int size)

{

int i;

if(start == size-1){

printf("%d, %d, %d \n", buff[0], buff[1], buff[2]);

return ;

}

for(i = start; i <= size -1; i++)

{

my\_swap(&buff[start], &buff[i]);

my\_per(start+1, size);

my\_swap(&buff[i], &buff[start]);

}

}

int main(int argc, char \* argv[])

{

//printf("%d\n", checkSquare(8));

buff[0]=1;

buff[1]=2;

buff[2]=3;

my\_per(0, 3);

}

코드 지져분해서 다시 정리..

#include <stdio.h>

#include <stdlib.h>

void my\_swap(int \* a, int \* b)

{

int tmp;

tmp = \*a;

\*a = \*b;

\*b = tmp;

}

void my\_per(int\* buff, int l, int r)

{

int i;

if(l == r){

printf("%d, %d, %d \n", buff[0], buff[1], buff[2]);

return ;

}

for(i = l; i <= r ; i++)

{

my\_swap(&buff[l], &buff[i]);

my\_per(buff, l+1, r);

my\_swap(&buff[i], &buff[l]);

}

}

int main(int argc, char \* argv[])

{

//printf("%d\n", checkSquare(8));

int buff[3]={1,2,3};

my\_per(buff, 0, 3-1);

}

geeks for geeks에 있는 코드  - [링크](https://www.geeksforgeeks.org/write-a-c-program-to-print-all-permutations-of-a-given-string/)

\