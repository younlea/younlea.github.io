---
title: "maximum-difference-between-two-elements"
excerpt_separator: "<!--more-->"
date: 2021-08-21 11:12:55 +0900
categories:
  - etc
tags:
  - AI
  - 머신러닝

toc : true
toc_sticky : true
---

훔.. 풀었었던건데 기억이 안나서 ㅡ.ㅡ;

나이가 들다보니 정리를 해 놔야 할듯하여 ㅜㅜ

ref : [https://www.geeksforgeeks.org/maximum-difference-between-two-elements/](http://https://www.geeksforgeeks.org/maximum-difference-between-two-elements/)

```
#include <unordered_map>
#include <algorithm>
#include <vector>
#include <stdio.h>
using namespace std;
 
 
void main()
{
    vector<int> a = { 5,4,32,1,6,8,100,2,3,4 };
 
    int max_diff = a[1] - a[0];
    int min_element = a[0];
    for (int i = 1; i < a.size(); i++)
    {
        if (max_diff < a[i] - min_element)
            max_diff = a[i] - min_element;
 
        if (min_element > a[i])
            min_element = a[i];
    }
 
    printf("%d %d", max_diff, min_element);
}
```

\