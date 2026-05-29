---
title: "binary에서 buffer에 쓰기."
excerpt_separator: "<!--more-->"
date: 2009-05-19 19:23:54 +0900
categories:
  - etc
tags:
  - etc

toc : true
toc_sticky : true
---

char\* mem;\
int max = 123123;\
int block = 64;\
int size;\
for (a = 0; a < max; a += block)\
{\
 size = a + block >= max ? max - a : block;\
 memcpy(buffer, &mem[a], size);\
 function(buffer, size);\
}