---
title: "임의 binary file 만들기."
excerpt_separator: "<!--more-->"
date: 2020-09-22 14:27:33 +0900
categories:
  - etc
tags:
  - etc

toc : true
toc_sticky : true
---

TEST code.. 간단히 file 만들어서 write 하는 코드입니다.

```
#include <stdio.h>

#define FULL 320*240

void main(void)

{

FILE *myimage;

myimage = fopen("./myimage.rgb", "w+b");

for(int i =0; i < FULL; i++)

fputc(i%255, myimage);

fclose(myimage);

}
```