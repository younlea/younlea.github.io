---
title: "coretex M33 register save."
excerpt_separator: "<!--more-->"
date: 2019-01-14 22:52:05 +0900
categories:
  - develop_env
tags:
  - develop_env
  - embedded
  - 임베디드

toc : true
toc_sticky : true
---

typedef struct MY\_REG{

unsigned long r0;

unsigned long r1;

unsigned long r2;

unsigned long r3;

unsigned long r4;

unsigned long r5;

unsigned long r6;

unsigned long r7;

unsigned long r8;

unsigned long r9;

unsigned long r10;

unsigned long r11;

unsigned long r12;

unsigned long r13;

unsigned long r14;

unsigned long r15; // PC

}MY\_REG;

MY\_REG my\_reg\_save;

void save\_reg\_data(MY\_REG \*save\_reg)

{

asm volatile(

"str r0, [%0, #0]\n\t" /\* R0 push to save\_reg \*/

"mov r0, %0\n\t" /\* R0 \*/

"str r1, [r0,#4]\n\t" /\* R1 \*/

"str r2, [r0,#8]\n\t" /\* R2 \*/

"str r3, [r0,#12]\n\t" /\* R3 \*/

"str r4, [r0,#16]\n\t" /\* R4 \*/

"str r5, [r0,#20]\n\t" /\* R5 \*/

"str r6, [r0,#24]\n\t" /\* R6 \*/

"str r7, [r0,#28]\n\t" /\* R7 \*/

"str r8, [r0,#32]\n\t" /\* R8 \*/

"str r9, [r0,#36]\n\t" /\* R9 \*/

"str r10, [r0,#40]\n\t" /\* R10 \*/

"str r11, [r0,#44]\n\t" /\* R11 \*/

"str r12, [r0,#48]\n\t" /\* R12 \*/

"str r13, [r0,#52]\n\t" /\* R13 - SP \*/

"str r14, [r0,#56]\n\t" /\* R14 - LR\*/

"sub r1, r15, #0x30\n\t" /\* PC \*/

"str r1, [r0,#60]\n\t"

"mrs r1, xpsr\n\t" /\* xPSR \*/

"str r1, [r0,#64]\n\t"

: //output

:"r"(save\_reg) //input

:"%r0","%r1" //chnanged value

);

}

/\*

inline volatile asm funcion.

1. asm

2. output

3. input

4. changed value

example : asm volatile("asm code" : output value : input value : changed value);

int test\_asm\_volatile(int input)

{

int output;

asm volatile("str r0, [%0]\n\t", output: "r"(input):"%r0");

}

\*/

참고

<http://jake.dothome.co.kr/inline-assembly/>

<http://www.ethernut.de/en/documents/arm-inline-asm.html>

<https://toanyone.net/arm-inline-asm>

<https://wiki.kldp.org/KoreanDoc/html/EmbeddedKernel-KLDP/app3.basic.html>

\