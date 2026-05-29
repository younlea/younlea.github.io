---
title: "typedef int(FUNC)(void) 기법 - by 별빛"
excerpt_separator: "<!--more-->"
date: 2005-09-08 09:02:25 +0900
categories:
  - etc
tags:
  - etc

toc : true
toc_sticky : true
---

#include 

typedef int (FUNC) (void);

int act1(void)\
{\
 return 100;\
}

int act2(void)\
{\
 return 200;\
}

void test(FUNC \*act)\
{\
 printf("R = [%d]\
", act());\
 return;\
}\
 \
int main(int argc, char \*\*argv)\
{\
 FUNC \*a = act1;\
 FUNC \*b = act2;

 test(a);\
 test(b) ;

 return 0;\
}

typedef int (FUNC) (void);는 FUNC가 int ()(void) 라는 것이다.

FUNC \*a; 이러면 a가 int를 리턴하고 void를 인자로 하는 함수의 포인터가 된는 것이다.

그래서 FUNC \*a = act1; 이것이 가능하다.

test() 함수는 FUNC \* 형을 인자로 받아서 실행한다. 위 프로그램 컴파일하고 실행하면 

R = [100] 

R = [200]\
이렇게 나온다.

C는 () 이것도 연산자이다. 우리가 함수 호출할때 a(); 이런식으로 하는 것을 볼 수 있다. \
C++ 에서 함수 객체 사용을 보면 () 연사자를 Test a = new Test(); 이렇게 오버로딩한다.\
함수를 저런식으로 gtk 에서도 많이 사용한다. 함수의 타입을 일일이 다 타이핑하고 있으면 넘 힘들므로.. 유닉스 시스템 프로그래밍에서 signal() 함수

void (\*signal(int signum, void (\*handler)(int)))(int);\
는 함수 포인터를 typedef 하면..\
void (\*signal(int signum, HANDLER)(int);

typedef void (\*isrp\_t)(unsigned long a);\
unsigned long a 를 인자로 받고 리턴값은 없는 형태의 함수를 함수포인터로 표현한 것이다. 그 앞에 typedef를 넣어 해당 타입을 isrp\_t 라는 이름으로 선언것이다. 즉 isrp\_t 라는건 위에 설명한 함수포인터라는 거다.

extern isrp\_t vecconnect (isrp\_t f, int lev0, int lev1);\
그래서 이것 또한 vecconnect 라는 함수가 외부모듈에 있다는 extern선언인데, 첫번째 인자가 위에서 선언한 함수포인터이다. 두번째 세번째는 int. 그리고 리턴값또한 isrp\_t 형식의 함수포인터이다.

결론은...\
함수포인터는 표현이 길고 복잡하므로 typedef로 간단하게 표기를 줄여서 짧고 간단하게 만든후, 다른 함수의 입력 인자와 리턴값의 형으로 써먹었다.\