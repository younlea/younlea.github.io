---
title: "Trie - array 이용해서 만들기"
excerpt_separator: "<!--more-->"
date: 2021-07-08 14:50:14 +0900
categories:
  - etc
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

malloc을 사용하지 않고 array를 사용해서 만들어 봤네요.

간단하게 trie로 구조를 만들고 제일 마지막 word 값을 root에 넣는 정도?

요걸 활용할 방법이 많아서 부모 노드 업뎃까지 살짝 짜봤네요.

```
#include <stdio.h>
 #define MAX_NODE 1000000
 
 struct Node{
     int word;
     int cnt;
     struct Node* pre;
     struct Node* ch[26];
 };
 
 struct Node node[MAX_NODE];
 int nCnt = 0;
 int wordCnt = 0;
 
 void init(void)
 {
     nCnt = 0;
     wordCnt = 0;
     for(register int i =0; i < MAX_NODE; i++)
     {
         node[i].word = 0;
         node[i].cnt = 0;
         node[i].pre = NULL;
         for(register int j =0; j < 26; j++)
             node[i].ch[j]=NULL;
     }
 }
 
 
 struct Node * getNode(void)
 {
    return &node[nCnt++];
 }
 
 void updateStr(struct Node* child)
 {
     if(child->pre == NULL)
         return;
 
     child->pre->cnt = child->cnt;
     updateStr(child->pre);
 }
 
 void addString(struct Node* root, char * str)
 {
     struct Node* cur = root;
 
     int i = 0;
     while(str[i] != '\0')
     {
         if(cur->ch[str[i]-'a']==NULL)cur->ch[str[i]-'a']=getNode();
         cur->ch[str[i]-'a']->pre = cur;
         cur= cur->ch[str[i]-'a'];
         i++;
     }
     cur->word = ++wordCnt;
     cur->cnt = wordCnt;
     updateStr(cur);
 }
 
 int max_len = 0;
 
 void myrec(struct Node* root, char *ans, int len)
 {
     if(root == NULL)
         return;
 
     struct Node* cur = root;
     if(cur->word > 0)
         printf("cur->word %d, len : %d %s\n", cur->word, len, ans);
 
     for(register int i = 0; i < 26; i++)
     {
        ans[len]= 'a' + i;
        myrec(cur->ch[i], ans, len+1);
     }
 
 }
 void recomendStr(struct Node* root, char * inp, char *ans)
 {
     struct Node* cur = root;
     int cnt =0;
     while(inp[cnt] != '\0')
         ans[cnt] = inp[cnt++];
 
     int i =0;
     while(inp[i]!= '\0')
     {
         if(cur->ch[inp[i]-'a']!= NULL)
             cur = cur->ch[inp[i]-'a'];
         i++;
     }
 
     myrec(cur, ans, cnt);
 
 }
 
 void main(void)
 {
     init();
 
     struct Node * root = getNode();
     addString(root, "abcd");
     addString(root, "bcde");
     addString(root, "abcdefghijklmnopqrstuvwxyz");
 
     printf("wordCnt : %d  nCnt : %d \n", wordCnt, nCnt);
 
     char str[100]={0,};
     recomendStr(root, "bc",str);
     printf("%d \n", root->cnt );
 }
 
```

\