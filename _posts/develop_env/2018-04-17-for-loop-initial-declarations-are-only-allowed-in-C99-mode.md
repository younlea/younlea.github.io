---
title: "for’ loop initial declarations are only allowed in C99 mode"
excerpt_separator: "<!--more-->"
date: 2018-04-17 23:18:04 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

gcc에서 작업하다 보면 아래와 같은 에러가 날때가 있습니다.

trie.c: In function ‘newNode’:

trie.c:15:3: error: ‘for’ loop initial declarations are only allowed in C99 mode

trie.c:15:3: note: use option -std=c99 or -std=gnu99 to compile your code

코드를 보면 아래 빨간 부분이 문제가 되는데..

15   for(int i=0; i<26; i++)

16     new->child[i]=0;

17   return new;

간단하게 int를 위에서 선언해 주면 해결 됩니다. ^^

14   int i =0; \

15   for(i ; i<26; i++)

16     new->child[i]=0;

17   return new;

\