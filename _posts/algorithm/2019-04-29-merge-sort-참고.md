---
title: "merge sort 참고. ^^;"
excerpt_separator: "<!--more-->"
date: 2019-04-29 10:50:16 +0900
categories:
  - algorithm
tags:
  - algorithm
  - 알고리즘

toc : true
toc_sticky : true
---

움.. 그냥 생각나서 다시 짜봤다.. merge sort.. ^^;

간단하다면 간단한데.. 분기 하면서 들어갈때 tmp를 통째로 카피하던 버릇이 있어서 이걸 필요한 만큼만 카피하게 했고..

tmp고 함수 안에서 만들고 했었는데.. 그냥 초기에 한번 선언해서 같이 사용하게 했다 ㅡ.ㅡ;

#include <stdio.h>

#include <string.h>

#include <stdlib.h>

int \* data = NULL;

int \* tmp = NULL;

void merge\_sort(int l, int r);

int main(int argc, char\* argv[])

{

int inputdata[11] ={5,10,1111,22,33,41,16,111,811,91,0};

data = inputdata;

tmp = calloc(1,sizeof(inputdata));

for(int i =0; i< 11; i++)

printf("%d,",data[i]);

merge\_sort(0, 10);

printf("\n");

for(int i =0; i< 11; i++)

printf("%d,",data[i]);

free(tmp);

return 0;

}

void merge\_sort(int l, int r)

{

if(r<=l) return;

int mid = (l+r)/2;

merge\_sort(l, mid);

merge\_sort(mid+1, r);

int ll, lr, idx;

ll= idx = l;

lr = mid+1;

memcpy(tmp+l, data+l, sizeof(int)\*(r-l+1));

while(ll<= mid && lr <=r)

{

if(tmp[ll]< tmp[lr]) data[idx++]=tmp[ll++];

else data[idx++]= tmp[lr++];

}

while(ll<=mid) data[idx++]=tmp[ll++];

while(lr<=r) data[idx++]= tmp[lr++];

}

\