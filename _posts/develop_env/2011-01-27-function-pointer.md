---
title: "function pointer..."
excerpt_separator: "<!--more-->"
date: 2011-01-27 15:32:38 +0900
categories:
  - develop_env
tags:
  - dataScience
  - datascience
  - 데이터

toc : true
toc_sticky : true
---

**기본적으로는 아래와 같이 쓴다.**

void (\*temp)(int);

void test\_1(int val)

{

...

}

void test\_2(int val)

{

...

}

void main(void)

{

temp = test\_1;

temp(100);   //-----------<< test\_1(100)

temp = test\_2;

temp(100);   // ----------<< test\_2(100)

}

그럼 **좀더 업그레이드 하면...**

아래처럼 function pointer를 typedef로 선언해서 함수를 사용하는 방법도 있다.

void (\*temp)(int val);

void test\_1(int val)

{

...

}

void test\_2(int val)

{

...

}

typedef void (\* fp )(int);

fp temp[2] = { test\_1, test\_2};

void main(void)

{

temp[0](100);  //<------------- test\_1(100);

temp[1](100);  //<------------- test\_2(100);

}

**생각을 바꿔서 이런것도 되나?? 된다.. 흐흐흐**

typedef void (\*fp)(void \*destaddr, void const \*srcaddr, size\_t len);

fp = memcpy; //<<-- void memcpy선언은 memcpy (void \*destaddr, void const \*srcaddr, size\_t len) 요렇게 되어 있다.

fp(dest\_addr, source\_addr, len);

결국 기본 타입이 같아서 저런식으로 써도 되는구나~~~ 이야.. 그런데 이런 방식은 잘 안쓸것 같다.

**좀더 진화가 되면 call back function을 등록해서 쓸때 사용하는데.. 흠..**

간단하게 설명하면.. 자 아래와 같다.

1. function pointer 선언..

typedef void(\*fp)(int);

2. 윗단에서 사용하는 함수 선언.

void highlayer(int temp);

3. low layer 함수 선언 밑 등록

fp lowlayer;

4. 함수 등록 함수.

void callbackfunction(fp cb)

{

lowlayer = cb;

}

5. callbackfunction(highlayer);

이렇게 등록후... lowlayer(..)를 호출하면 highlayer(...)가 호출되게 됩니다.\
결국 highlayer()는 다른사람이 만들고 우리는 lowlayer()를 호출만 하면 되는게 되죠.

말이 잘 안되네..

사용하는 용도로는 보통 윗단에서 데이터 receive 함수를 만들어 놓고 데이터가 오기를 기다린다.

그리고 밑에단에서는 데이터가 왔을때 윗단 함수를 호출해 줘야 하는데..

자.. 여기서 extern으로 끌어서 함수 호출해도 된다. 그런데 이러면 library 화가 힘드니

중간 단에서 연결해줄수 있는 포인트를 만들어주면 여러곳에서 쓸수 있게 된다는것이다.

아 말 참.. ㅜㅜ

\