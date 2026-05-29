---
title: "JAVA 형변환 (int, float, double &lt;-&gt; string)"
excerpt_separator: "<!--more-->"
date: 2012-04-05 23:17:55 +0900
categories:
  - develop_env
tags:
  - develop_env
  - embedded
  - 임베디드

toc : true
toc_sticky : true
---

**숫자를 문자열로 바꾸기**

int i = 1234;\
String s = String.valueOf(i);     문자열 "1234"로 변환\
String s = Integer.toString(i);   문자열 "1234"로 변환\
String s = ””+i;                문자열 "1234"로 변환\
String s = “”+12.34;            문자열 "12.34"로 변환\
String s = “”+0;                문자열 "0"로 변환\

**문자열을 숫자로 바꾸기**\
String str = "1234";\
int i = Integer.valueOf(str).intValue();\
int i = Integer.parseInt(str);\
long i = Long.parseLong(str)\
double i = Double.valueOf(str).doubleValue();\
Byte.parseByte(str)        바이트형 정수로 변환 \
Short.parseShort(str)      short형 정수로 변환 \
Integer.parseInteger(str)  int형 정수로 변환 \
Long.parseLong(str)        long형 정수로 변환 \
Float.parseFloat(str)      float형 부동 소수로 변환 \
Double.parseDouble(str)    double형 부동 소수로

**문자 16진수 → 숫자 10진수**

String strHexValue = "A";\
System.out.println(Integer.parseInt(strHexValue, 16));

>> 10\

**숫자 10진수 → 문자 16진수**

System.out.println(Integer.toString(intHexValue, 16));

>> a\

**소문자 → 대문자**

String strSmallA = "a";\
String strLargeA = strSmallA.toUpperCase();\

**문자 비교**

"a".equals("A") → false\
"a".equalsIgnoreCase("A") → ture