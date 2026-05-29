---
title: "memmove and memcpy에 대한 고찰~"
excerpt_separator: "<!--more-->"
date: 2005-08-08 16:42:49 +0900
categories:
  - etc
tags:
  - dataScience
  - datascience
  - 데이터

toc : true
toc_sticky : true
---

#include \
#include 

#ifdef \_\_\_\_\_\_\_\_what\_\_\_\_\_\_\_\_\_

 이번에 테스트 하는것은 memcpy와 memmove에 대한 테스트 이다. \
 우선 첫째로 순차적으로 저장되어 있는 배열을 사용하여\
 memmove와 memcpy를 테스트하였다. \
 우선 간단한 설명을 하면\
 memmove(a,b,c)는 b번째 주소부터 c크기만큼의 내용을 a번째 주소까지 옮기라라는것이다.\
 memcpy(a,b,c)는 b번째 주소부터 c크기 만큼의 내용을 a번째 주소부터 카피하라는 말이다. 

 자세히 보면 카피나 무브나 비슷하다라는것을 알수 있다. 

 그런데 memmove한 곳의 데이터는 남아있을까? =>> move한 후에도 데이터는 남아있다. \
 그리고 memcpy 한곳읜 데이터는 남아 있을까? ==> 데이터를 위에 덧씨우고 자신은 남아있다. \
 궁굼하지 않은가? 허허 해보자구..

 음 결국 \
 memcpy(str+8,str2,strlen(str2)); == memmove(str+8,str2,strlen(str2));\
 이런 결과가 나오는것 같다.. 허허허.. 재미 있었남?

 오호.. memset라는 함수도 있군.... \
 이것은 초기화를 해줄때 아주 유용한것 같다. \
 memset(a,b,c)는 a번째 주소부터 c크기 만큼의 데이터를 b로 초기화 해주는것인다.\
 \
#endif\
#define \_\_memset\_\_ 

int main(int argc, char \*argv[])\
{

 char str[32]="You are beautiful";

 char str2[]="very ";

#ifdef \_\_memset\_\_

int ar[10];\
int count=0;\
memset(ar,0,sizeof(ar));\
for(count ; count< sizeof(ar)/4; count++)\
printf("%d",ar[count]);

#endif //end \_\_memset\_\_

 

 memmove(str+13,str+8,10);\
 printf("\
memmove 한후 str을 확인한다. ");\
 puts(str);

 memcpy(str+8,str2,strlen(str2));\
 //memmove(str+8,str2,strlen(str2));\
 printf("memcpy 한후 str2를 확인한다.");\
 puts(str2);

 printf("\
");\
 puts(str);

return 0;

}\