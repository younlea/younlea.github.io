---
title: "hash table 만들어 보기"
excerpt_separator: "<!--more-->"
date: 2021-07-10 10:30:43 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

hash 관련 살짝 정리 ㅡ.ㅡ;

```
#include <stdio.h>
 
 #define MAX_CNT 30000
 
 typedef struct HASH_NODE{
     char *str;
     int key;
 }hashNode;
 
 hashNode myhash[MAX_CNT];
 
 long long my_hash(char * str)
 {
     long long ret = 0;
 
     int i  = 0;
     while(str[i] != 0)
     {
         ret = (ret << 5) + str[i++] - 'a' +1;
     }
 
     return ret;
 }
 
 void add_hash(char * str)
 {
     long long key = my_hash(str);
     int k_cnt = key % MAX_CNT;
     int cnt = 0;
     while(cnt++ != MAX_CNT)
     {
         if(myhash[k_cnt].key == 0)
         {
             myhash[k_cnt].key = key;
             break;
         }else if(myhash[k_cnt].key != key)
         {
             k_cnt = (k_cnt + 1)% MAX_CNT;
         }
     }
 }
 
 int find_hash(char * str)
 {
     long long key = my_hash(str);
     int k_cnt = key % MAX_CNT;
     int cnt = 0;
     int ret;
     while(cnt++ != MAX_CNT)
     {
         if(myhash[k_cnt].key == key)
             return k_cnt;
         else if(myhash[k_cnt].key == 0)
             return -1;
         else
             k_cnt = (k_cnt +1) %MAX_CNT;
 
     }
 
 }
 
 void main(void)
 {
     hashNode tmp;
     tmp.str = '\0';
     tmp.key = 0;
 
     for(int i = 0; i < MAX_CNT; i++)
         myhash[i] = tmp;
 
     add_hash("abcd");
     add_hash("cdef");
     add_hash("qwer");
 
     printf("%d \n", find_hash("abcd"));
     printf("%d \n", find_hash("ccef"));
     printf("%d \n", find_hash("qwer"));
 
 }
 
 
```

\