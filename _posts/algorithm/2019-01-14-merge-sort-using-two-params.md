---
title: "merge sort using two params."
excerpt_separator: "<!--more-->"
date: 2019-01-14 23:47:10 +0900
categories:
  - algorithm
tags:
  - algorithm
  - 알고리즘

toc : true
toc_sticky : true
---

인자 두개를 소팅 하는 방법을 구현해 보았습니다.

a인자를 먼저 쏘팅하고 a인자가 같으면 b인자를 기준으로 했습니다.

#include <stdio.h>

#include <memory.h>

typedef struct MY\_DB{

int a;

int b;

}MY\_DB;

MY\_DB sorting\_test[10000];

MY\_DB sorting\_buff[10000];

int sorting\_count = 0;

void insert\_db(int a, int b)

{

sorting\_test[sorting\_count].a = a;

sorting\_test[sorting\_count++].b = b;

}

void merge\_sort(int l, int r)

{

int i = 0;

if(r - l <=0)

return;

int idx, mid, ll, lr;

mid = (l+r)/2;

merge\_sort(l, mid);

merge\_sort(mid+1, r);

idx = ll = l;

lr = mid+1;

for(i = l; i <=r; i++)

{

sorting\_buff[i] = sorting\_test[i];

}

while(ll <= mid && lr<=r)

{

if(sorting\_buff[ll].a<sorting\_buff[lr].a)

{

sorting\_test[idx++] = sorting\_buff[ll++];

}

else if(sorting\_buff[ll].a>sorting\_buff[lr].a)

{

sorting\_test[idx++] = sorting\_buff[lr++];

}else // a 같을 경우

{

if(sorting\_buff[ll].b<=sorting\_buff[lr].b)

{

sorting\_test[idx++] = sorting\_buff[ll++];

}

else if(sorting\_buff[ll].b>sorting\_buff[lr].b)

{

sorting\_test[idx++] = sorting\_buff[lr++];

}

}

}

while(ll<=mid)

{

sorting\_test[idx++] = sorting\_buff[ll++];

}

while(lr<=r)

{

sorting\_test[idx++] = sorting\_buff[lr++];

}

}

int main(int argc, char \*argv[])

{

int i = 0;

memset(sorting\_test, 0, sizeof(sorting\_test));

memset(sorting\_buff, 0, sizeof(sorting\_buff));

insert\_db(1,5);

insert\_db(1,2);

insert\_db(1,3);

insert\_db(2,7);

insert\_db(3,3);

insert\_db(2,3);

insert\_db(1,1);

insert\_db(2,1);

insert\_db(5,11);

printf("before original \n");

for( i = 0; i<sorting\_count; i++ )

printf("[%d],(a,b)= (%d, %d)\n", i, sorting\_test[i].a, sorting\_test[i].b);

merge\_sort(0, sorting\_count-1);

printf("----after sorting----\n");

for( i = 0; i<sorting\_count; i++ )

printf("[%d],(a,b)= (%d, %d)\n", i, sorting\_test[i].a, sorting\_test[i].b);

}