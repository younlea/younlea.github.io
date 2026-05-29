---
title: "stl - vector 사용"
excerpt_separator: "<!--more-->"
date: 2021-08-20 10:00:07 +0900
categories:
  - algorithm
tags:
  - AI
  - 머신러닝

toc : true
toc_sticky : true
---

배열을 동적으로 할당해주는 녀석으로 C로 짜는 걸 쉽게 해준다 정도로 이해하면 될 듯

일단 정리하고 나중에 C랑 비교해서 정리해 보겠음. 그게 이해가 더빠를듯.. (C에 익숙하다면 ^^)

사용을 위해 vecotr를 include 해야 함

#include  <vector>

선언

// vector <type> name;

vector <int> v;

vector 추가 (변수 추가)

v.push\_back(value);

v.insert(index, value);     //index 번호에 value를 넣어줌. index 이후의 데이터는 뒤로 자동으로 미뤄줌

vector<int>::iterator it = v.begin();

v.insert(it, value); //제일 앞에 넣는다.

vector 값 삭제

v.pop\_back();   // 제일 뒤에 값을 삭제해줌

v.erase(index);   // index의 값을 지우고 앞으로 땡김

v.erase(index1, index2)   //index1~ index2까지 지우고 땡김

v.clear(); // 모두 삭제(alloc되어 있는 공간은 그대로임)

vector 크기 확인

v.size(); 실제 들어 있는 변수의 크기

v.capacity ; 할당되어 있는 메모리 size

vector값 출력

v[i];  // i에 바로 접근.. 해당 접근을 위해서는 v.size()보다 작은값만 접근해야 함.

v.at(i);  // v.size()체크할 필요없이 접근가능, 대신 약간 느림.

v.front();  //처음 요소

v.back(); //마지막 요소

예제 : vector and sort

```
#include <unordered_map>
#include <algorithm>
#include <vector>
#include <stdio.h>
using namespace std;
 
 
// vector test
void main()
{
 
    vector <int> v;
 
    v.push_back(1);
    v.push_back(2);
    v.push_back(5);
    v.push_back(7);
    v.push_back(3);
    printf("front : %d \n", v.front());
    printf("end : %d \n", v.back());
 
    sort(v.begin(), v.end());
 
    for (int i = 0; i < v.size(); i++)
        printf("%d ", v[i]);
 
    v.clear();
}
```

ref1 : <https://blockdmask.tistory.com/70>

ref2: [https://coding-factory.tistory.com/596](http://https://coding-factory.tistory.com/596)