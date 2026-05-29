---
title: "Linkedlist"
excerpt_separator: "<!--more-->"
date: 2021-07-01 09:34:05 +0900
categories:
  - etc
tags:
  - algorithm
  - 알고리즘

toc : true
toc_sticky : true
---

그냥 배열 써서 링크드 리스트 미리 할당하고 쓰는거 짜봤네요 ㅡ.ㅡ;

은근히 맨날 헷깔린다는 ^^;

```
#include <stdio.h>
 #define nullptr '\0'
 #define MAX_NODE 1000
 
 typedef struct NODE{
     int data;
     struct NODE* next;
 }Node;
 
 Node node[MAX_NODE];
 int nodeCnt;
 Node* head;
 
 Node* getNode(int data){
     node[nodeCnt].data = data;
     node[nodeCnt].next = nullptr;
     return &node[nodeCnt++];
 }
 
 void init(void)
 {
     Node temp;
     temp.data = 0;
     temp.next = nullptr;
 
     for(int i =0; i< MAX_NODE; i++)
         node[i] = temp;
 }
 
 //insert front 
 void addNode2Head(int data)
 {
     if(nodeCnt > MAX_NODE)
         return;
 
     Node* temp = getNode(data);
 
     temp->next = head;
     head = temp;
 }
 
 // insert last 
 void addNode2Tail(int data)
 {
     if(nodeCnt > MAX_NODE)
         return;
 
     Node* cur = head;
     
     if(cur == nullptr)
         head=getNode(data);
     
     Node* pre = cur;
     while(cur->next != nullptr)
     {   
         pre = cur;
         cur = cur->next;
     }   
     Node * tmp =getNode(data);
     tmp->next = cur;
     pre->next = tmp;
 }
 void removeNode(int data)
 {
     Node* cur = head;
     Node* pre = nullptr;
 
     if(cur == nullptr)
         return;
 
     while(cur!=nullptr)
     {
         if(cur->data == data)
         {
             if(pre == nullptr)
                 head = cur->next;
             else
                 pre->next = cur->next;
         }
         pre = cur;
         cur = cur->next;
     }
 }
 
 void printll(void)
 {
     Node* cur = head;
 
     while(cur)
     {
         printf("%d \n", cur->data);
         cur = cur->next;
     }
 }
 
 void main(void)
 {
     init();
 
     head = getNode(-1);
 
     addNode2Head(1);
     addNode2Head(2);
     addNode2Head(3);
 
     printll();
     printf("--------\n");
 
     addNode2Tail(4);
     addNode2Tail(5);
 
     printll();
 
 
     printf("--------\n");
     removeNode(3);
     removeNode(-1);
     removeNode(1);
     printll();
 }
 
 
```

훔 위에 꺼는 linked list의 head를 갈아 치는건데 이러면 remove할때 생각할게 많아진다. 다른 방법으로 head->next 에 계속 붙이는 방법이 있는데add 할때ll \*tmp = alloc.. tmp ->next = head->next;head ->next = tmp;로 하면 되고 \
tail에 붙혀도..ll \* cur = head;while(cur->next != nullptr) cur= cur->next;ll \*tmp = getnode();cur->next = tmp;\
remove할때도 ll \* pre = nullptr;ll \* cur = head;while(cur->next != nullptr){pre = cur;cur = cur->next;if(cur->next == data){pre->next = cur->next;return;}}\

요렇게 간단해 진다 ㅡ.ㅡ;

움 머리가 굳은건지..

\