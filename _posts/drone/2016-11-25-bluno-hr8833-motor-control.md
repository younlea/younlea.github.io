---
title: "bluno + hr8833 - motor control"
excerpt_separator: "<!--more-->"
date: 2016-11-25 00:02:45 +0900
categories:
  - drone
tags:
  - robot
  - 로봇

toc : true
toc_sticky : true
---

bluno beetle

<https://www.dfrobot.com/wiki/index.php/Bluno_Beetle_SKU:DFR0339>

![image](/assets/images/posts/6069992/c0028014_5836ffc659a43.png)

D3, D5 is PWM

D2, D4 is GPIO out

HR8833

<https://www.dfrobot.com/wiki/index.php/HR8833_Dual_DC_Motor_Driver_(SKU:DIR0040)>

![image](/assets/images/posts/6069992/c0028014_5836ffe0268c5.png)

D2          -- IB2

D3(PWM)   -- IB1

D4          -- IA2

D5(PWM)   -- IA1

```
/*
* @file Motor driver HR8833-Test.ino
* @brief HR8833-Test.ino  Motor control program
*
* control motor positive inversion
* 
*/
const int IA1=5;
const int IA2=4;
const int IB1=3;
const int IB2=2;
 
void setup() {
     pinMode(IA1, OUTPUT);
     pinMode(IA2, OUTPUT);
     pinMode(IB1, OUTPUT);
     pinMode(IB2, OUTPUT);
}
 
void loop() {
 MA1_Forward(200);//MA 1 road motor is transferred, PWM speed control
 MB1_Forward(200);
 delay(1000);
 //MA2_Backward(200);//MA 2 road motor is transferred, PWM speed control
 //delay(1000); 
}
 
void MA1_Forward(int Speed1)
{
     analogWrite(IA1,Speed1);                                                      
     digitalWrite(IA2,LOW);  
  }
  
void MA2_Backward(int Speed1)
{    
    int Speed2=255-Speed1;
    analogWrite(IA1,Speed2);
    digitalWrite(IA2,HIGH); 
  }
  
void MB1_Forward(int Speed1)
{
     analogWrite(IB1,Speed1);
     digitalWrite(IB2,LOW);  
  }
  
void MB2_Backward(int Speed1)
{    
    int Speed2=255-Speed1;
    analogWrite(IB1,Speed2);
    digitalWrite(IB2,HIGH);   
  }
```

![image](/assets/images/posts/6069992/c0028014_58370372bce3a.jpg)

![image](/assets/images/posts/6069992/c0028014_5863ddaf1b657.jpg)

\