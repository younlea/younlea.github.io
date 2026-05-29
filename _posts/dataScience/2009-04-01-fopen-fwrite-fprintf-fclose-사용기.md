---
title: "fopen fwrite fprintf fclose 사용기~~~"
excerpt_separator: "<!--more-->"
date: 2009-04-01 21:03:49 +0900
categories:
  - dataScience
tags:
  - dataScience
  - datascience
  - 데이터

toc : true
toc_sticky : true
---

훔.... 일을 하다가....

파일을 encrpiton해서 다시 받은쪽에서 decription을 하고 그걸 다운로드하는 코드를 리뷰중이다...

하다보니.. 결국 fopen. fwrite. fclose 함수를 사용하더군...

결국 제일 아래까지 내려가야한다... MFC는 MFC일뿐... 알고리즘이 중요하고... 기본이 중요하게 된다.. 허허..

아래 찾다보니 정리 잘해놓으신 분이 계셔서 올려본다. 훔..

fwrite 와 fprint 의 차이점은? [출처 <http://soyoja.com/292>]

fwrite() & fprintf() -- binary or text??

우선, 저수준함수인 fwrite 를 오버라이딩해서 파라미터에 따른 서식에 맞게 보다 편리하게 출력할 수 있도록 만든 것이 fprintf 이다.

위의 링크 글에 설명되었듯이, fwrite 는 버퍼에 있는 내용을 그대로 다 출력한다. 반면에 fprintf 는 표준출력 (Standard output stream) 모드로 동작하여 일반적인 문자열 버퍼 출력 방식을 따르게 된다. 즉 '/0' 과 같은 문자열 종료 캐릭터를 만나면 출력을 종료한다.

아래 예제를 보면 명확하다.

view plaincopy to clipboardprint?\
#include <iostream>   \
  \
using namespace std;   \
  \
int main()   \
{   \
    FILE\* f1 = fopen("file1.txt", "w");   \
    FILE\* f2 = fopen("file2.txt", "w");   \
  \
    char\* data = "HELLO\0WORLD";   \
    int len = 11;   \
  \
    fwrite(data, 1, len, f1);   \
    fprintf(f2, data);   \
  \
    return 0;   \
}  \
fprintf 를 사용하여 출력하는 경우 HELLO 까지만 찍고, 문자열 종료 널문자 '\0' 을 만났기 떄문에 출력을 종료한다. \
반면에 fwrite 는 '\0' 와 상관없이 버퍼 전체의 내용을 그대로 출력해서 HELLO\0WORLD 를 모두 출력한다. ( 텍스트 파일에서는 HELLO WORLD 로 보임 )

결국 printf 서식에 맞게 문자열 출력방식으로 출력하고 싶으면 fprintf 를 쓰고, 버퍼에 있는 모든 내용 혹은 버퍼내의 특정 범위의 내용을 그대로 출력하고자 할 때는 fwrite 를 쓰면 된다.

아... fwrite 예제를 짜서 올린 분이 있더군 [출처][http://blog.paran.com/dct/28042997]

fwrite를 이용한 파일에 내용 써넣기 예제..

#include <stdio.h>\
#include <string.h>\
#include "common.h"

void setValue(char \*buf)\
{\
        char tempBuf[64];\
        memset(tempBuf, 0x00, sizeof(char) \* 64);

        int a = 0;\
        \
       sprintf(tempBuf, "test %d\n", a);\
        strcat(buf, tempBuf);\
        \
        sprintf(tempBuf, "test2 %d\n", a+1);\
        strcat(buf, tempBuf);\
        \
        sprintf(tempBuf, "\n%d\t%d\n%d\t%d\n\n", a+1, a+2, a+3, a+4);\
        strcat(buf, tempBuf);\
        \
        sprintf(tempBuf, "test3 %2d\n", a+5);\
        strcat(buf, tempBuf);\
        \
        for(int idx = 0; idx < 50; idx++)\
        {\
                sprintf(tempBuf, "use the for-statement %2d\n", idx);\
                strcat(buf, tempBuf);\
        }

}

int main( void )\
{\
        FILE \*fpRW;

        char writeBuf[4096];\
        memset(writeBuf, 0x00, sizeof(char) \* 4096);

        setValue(writeBuf);

        fpRW = fopen(".\\test.txt", "w+");\
        \
        fwrite(writeBuf, sizeof(char), strlen(writeBuf), fpRW);\
        \
        fclose(fpRW);

        return 0;\
}

모 결국 난...\
fopen\
fwrite\
fclose 이렇게 쓰는군...

헉.. 또 좋은 자료가...[출처]http://blog.naver.com/sshuikr?Redirect=Log&logNo=100060144346

size\_t fread(void \*ptr, size\_t size, size\_t n, FILE \*stream);

size\_t fwrite(const void \*ptr, size\_t size, size\_t n, FILE \*stream);

fread() 와 fwrite() 의 인자는 같습니다.

\*ptr          : 첫번째 인자로써 파일에서 읽은 데이터를 저장할 메모리를 넘깁니다.\
                  아래 예제에서는 하나의 문자씩 읽을거니까 char형 변수의 주소를 넘겨주면 되겠죠.\
size         : 두번째 인자는 읽을 단위를 넘깁니다.

                  char형을 읽을 거니까 char는 1바이트, 그래서 1이라고 두번째 인자를 씁니다.\
n           : 세번째인자는 두번째 인자로 넘긴 크기를 몇번 읽을 것인가

                  1바이트를 한번만 읽을거니까 1이 세번째 인자로 사용됩니다.\
\*stream  : 마지막으로, 어디서 읽을 것인가를 FILE\*로 지정합니다.\
                  fp\_in에서 읽어야겠죠.

훔.. 보다 보니.. fseek... 역시나 잘 설명해 놓으신 분이 있군.. ㅡ.ㅡ;\
[출처]http://blog.naver.com/tazamara?Redirect=Log&logNo=120013612367

/\* fseek 파라메터 중 SEEK\_SET, SEEK\_CUR, SEEK\_END 사용법 \*/

#include <stdio.h> \
#include <stdlib.h>\
#include <time.h>

void main(void) \
{

 int  array[100]={0}; \
 int  array2[100]={0}; \
 int  value,  value2,  value3, value4; \
 long  temp; \
 FILE  \*fp;

 srand(  (unsigned)time(  NULL  )  );

 for(int i=0;  i<100;  i++) \
  array[i]=rand()%1000;

 if  ((  fp=fopen("RAND.txt",  "wb"))  ==  NULL)   \
 { \
  printf("file  open  error  to  write"); \
  exit(1); \
 }

 if  (fwrite(array,  sizeof(array[0]),  100,  fp)  ==  NULL  )   \
  // 정수를 파일에 저장하고픈데... sizeof(array) 하면 저수 포인터 \
  // 의 크기를얻어 오는 거겠죠... 이는 적당하지 않습니다. \
  // 차라리 아래의 sizeof(int) 가 낳죠.. 그래야 쓰고자 하는 \
  // 정확한 byte를 쓰겠죠... \
  // 포인터 크기와 정수 포인터 크가기 같다면 별문제없이 동작할것입니다. \
  // 게다가 쓰고자 하는 개수는 1 이 아니라 100 개 겠죠... \
 { \
  printf("file  write  error"); \
  exit(1); \
 }

 fclose(fp);

 if  ((  fp=fopen("RAND.txt",  "rb"))  ==  NULL)   \
 { \
  printf("file  open  error  to  read"); \
  exit(1); \
 }

 fread(array2,  sizeof(array[0]),  100,  fp); \
 // 이곳도 마찬가지...

 for  (int j=0;  j<100;  j++) \
  printf("%d\t",  array2[j]);

 printf("\n");

 /\* 처음의 위치의 값 \*/\
 fseek(fp,  0,  SEEK\_SET); \
 fread(&value,  sizeof(array[0]),  1,  fp); \
 // 이곳을 잘 보세요....int 라는 것도 좋습니다. 다만... 이것은 \
 // int 라는 것을 항상 알고있어야 겠죠... \
 // 만약 struct 라든지 다른 것이 생긴다면....이는 그 한 원소를 \
 // 선택해서 크기를 산출해 내는 것이 좋습니다. \
 // sizeof(int) 보다는 sizeof(array[0]) 가 낳습니다. \
 fprintf(stdout,  "%d"  ,  value);

 /\* 처음 위치에서 8바이트만큼 이동한 위치 \*/

 fseek(fp,  sizeof(array[0])\*2,  SEEK\_CUR); \
 // 만약 이게 정수 int 가 아닌 char 문자 였다면 별로 문제 없이 \
 // 님이 의도한 대로 읽혔을 겁니다. \
 // 하지만 int 는 아니죠.. 그 원인은 int 한 변수의 크기는 1byte 단위가 \
 // 아니라 컴터 마다 다르겠지만 적어도 2byte 이상은 하죠... \
 // 따라서 컴터에서의 정수 표현 byte를 구해와 그 수에 정수배를 해주성 \
 // 앞 혹은 뒤로 이동을 해 주어야 합니다. \
 fread(&value2,  sizeof(array[0]),  1,  fp);  // array[3]\
 fprintf(stdout,  "  %d"  ,  value2);

/\* 2바이트 만큼 이동한 위치와 동일한 위치 설정 : 처음 부터 12Byte \*/

 fseek(fp,  sizeof(array[0])\*3,  SEEK\_SET); \
 fread(&value4,  sizeof(array[0]),  1,  fp);   // array[3]\
 fprintf(stdout,  "  %d"  ,  value4);

 /\* 마지막 위치의 값 \*/\
 fseek(fp,  sizeof(array[0])\*(-1),  SEEK\_END); \
 // 파일의 맨 마지막으로 이동한다면.. 아무 값도 없겠죠.. \
 // 파일의 끝은 말 그대로 끝입니다. 더 이상의 데이타는 없답니다. \
 // 따라서 끝에서 마지막 정수를 읽기 위해서는 한 칸 전진해서 일거야 \
 // 겠죠... 그래서 -1 해준거랍니다. \
 fread(&value3,  sizeof(array[0]),  1,  fp); \
 fprintf(stdout,  "  %d"  ,  value3);

 printf("\n");

 fclose(fp); \
}

훔.. 결국.. 직접 해봐야것다.. ㅋㅋ \
회사와서 어느덧.. 5년이 지났는데.. 아는건? 몰까????\
하다보면 될까나 ^^; 하이튼 힘내자..