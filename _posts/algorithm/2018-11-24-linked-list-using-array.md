---
title: "linked list using array."
excerpt_separator: "<!--more-->"
date: 2018-11-24 23:15:02 +0900
categories:
  - algorithm
tags:
  - algorithm
  - 알고리즘

toc : true
toc_sticky : true
---

//I will make a linked list.

//init linked\_list

//add node

//remove node using data

//remove node using idx

#include <stdio.h>

#include "memory.h"

#define MAX\_SIZE 10000

enum test{

ADD = 1,

REMOVE\_ID,

REMOVE\_DATA,

PRINT,

};

typedef struct Node{

int idx;

int data;

int next;

};

Node node[MAX\_SIZE];

int rear;

void Init\_linked\_list()

{

memset(node, -1, sizeof(node));

node[0].next = 1;

rear = 0;

}

void add\_linkedlist(int idx, int data)   //data가 작은 순서대로 , 같으면 idx가 작은 순서대로

{

int pos = 0;

int cur = node[0].next;

rear++;

while (node[cur].idx != -1 && cur != -1)

{

if (node[cur].data > data)  //search insert point

{

node[pos].next = rear;

node[rear].idx = idx;

node[rear].data = data;

node[rear].next = cur;

return;

}

if (node[cur].data == data)  //search insert point

{

if (node[cur].idx > idx)

{

node[pos].next = rear;

node[rear].idx = idx;

node[rear].data = data;

node[rear].next = cur;

return;

}

if (node[cur].idx < idx)

{

node[rear].idx = idx;

node[rear].data = data;

node[rear].next = node[cur].next;

node[cur].next = rear;

return;

}

}

pos = cur;

cur = node[cur].next;

}

node[pos].next = rear;

node[rear].idx = idx;

node[rear].data = data;

}

void print\_linked()

{

int idx = 1;

int pos = 0;

printf("\nprint list \n");

while (node[pos].next != -1)

{

pos = node[pos].next;

printf("#%d  idx[%d]  data[%d] \n", idx, node[pos].idx, node[pos].data);

idx++;

}

}

void remove\_idx(int idx)

{

int cur = 0;

int pos = 0;

printf("\nremove idx [%d]\n", idx);

while (node[cur].next != -1)

{

pos = cur;

cur = node[cur].next;

if (node[cur].idx == idx)

{

node[pos].next = node[cur].next;

}

if (node[cur].idx > idx)

return;

}

}

void remove\_data(int data)

{

int cur = 0;

int pos = 0;

printf("\nremove data [%d]\n", data);

while (node[cur].next != -1)

{

pos = cur;

cur = node[cur].next;

if (node[cur].data == data)

{

node[pos].next = node[cur].next;

cur = pos;

}

if (node[cur].data > data)

return;

}

}

int main(void)

{

int test\_case;

scanf("%d", &test\_case);

int action;

int idx;

int data;

Init\_linked\_list();

for (int i = 0; i < test\_case; i++)

{

scanf("%d", &action);

if (action == ADD) //insert ^^;

{

scanf("%d %d", &idx, &data);

add\_linkedlist(idx, data);

}

if (action == REMOVE\_ID)   //remove data

{

scanf("%d", &idx);

remove\_idx(idx);

}

if (action == REMOVE\_DATA)

{

scanf("%d", &data);

remove\_data(data);

}

if (action == PRINT)  // id, data from head to tail

{

//print linked lost

print\_linked();

}

}

return 0;

}

------------------------------------------------------

// input data

------------------------------------------------------

15

1 0 8

1 1 5

1 2 9

1 3 1

1 4 11

1 5 78

1 6 1

1 7 7

1 8 9

1 9 1234

4

2 9

4

3 9

4

\