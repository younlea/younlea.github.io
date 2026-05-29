---
title: "linked list using array"
excerpt_separator: "<!--more-->"
date: 2018-11-23 22:41:53 +0900
categories:
  - algorithm
tags:
  - algorithm
  - 알고리즘

toc : true
toc_sticky : true
---

#include <stdio.h>

struct Node{

int to, cost;

Node \*next;

}

int nidx;

Node nodes[1000001];

---------------------------

#define DIV 200;

struct Node{

int data;

int next;

}

int Head[100000/DIV+5]

Node List[100005];

int M;

void add(int val)

{

List[M].data = val;

List[M].next = Head[val/DIV];

Head[val/DIV] = M++;

}

void remove\_list(int val)

{

int prev = 0;

int cur = Head[val/DIV];

while(cur)

{

if(List[cur].data == val)

{

if(prev == 0)

Head[val/DIV]=List[cur].next;

else

List[prev].next = List[cur].next;

break;

}

prev = cur;

cur= List[cur].next;

}

}

void find\_val(int val)

{

}

int main(void)

{

M = 1;

memset(Head, -1, sizeof(Head));

}

int gueue[100005], front, rear;

bool flag[100005];

bool dir;