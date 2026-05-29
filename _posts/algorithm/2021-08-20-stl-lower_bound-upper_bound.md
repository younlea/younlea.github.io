---
title: "stl -  lower_bound, upper_bound"
excerpt_separator: "<!--more-->"
date: 2021-08-20 11:18:07 +0900
categories:
  - algorithm
tags:
  - algorithm
  - 알고리즘

toc : true
toc_sticky : true
---

C에서 binary search를 하는데.. 이게 stl에 함수가 있어서 정리 한다.

일단 둘다 오름차순으로 정렬이 되어 있어야 한다. 1,2,3,4,5, 이렇게 ^^;

lower\_bound : 찾으려는 숫자보다 같거나 큰 숫자 (이상)가 시작되는 위치

upper\_bound : 찾으려는 숫자보다 큰 숫자(초과)가 시작되는 위치

#include <algorithm>

//배열의 경우

int a[10] = {0,1,2,3,4,5,6,7,8,9};

// 6 이상되는 숫자가 시작되는 위치

lower\_bound(a,a+10, 6) - a;

//6보다 큰 숫자가 시작되는 위치

upper\_bound(a, a+10, 6) - a;

lower\_bound의 반환형은 Iterator 이므로 실제로 몇 번째 인덱스인지 알고 싶다면,

위 코드와 같이 lower\_bound 값에서 배열 첫 번째 주소를 가리키는 배열의 이름을 빼 주면 됩니다.

//vector의 경우

vector <int> v = {0,1,2,3,4,5,6,7,8,9};

lower\_bound(v.begin(), v.end(), 6) - arr.begin();

움 일단 숫자에 대한 비교는 그런데.. 만약에 아래와 같이 특정 type으로 되어 있는 녀석이면 compare 함수를 선언하면 됨

typedef struct DATA{

int idx;

char \* str;

}data;

data d[10];

.....

bool compare(data a, data b)

{

if(a.idx < b.idx) return true;

else return false;

}

lower\_bound(data, data+10, 3, compare);

참고용 C 코드

```
#include <unordered_map>
#include <algorithm>
#include <vector>
#include <stdio.h>
using namespace std;
 
// upperbound(초과), lower bound(이상)
void main()
{
    int a[10] = { 0,1,5,6,7,45,56,67,78,89 };
 
    int idx = 0;
    idx = lower_bound(a, a + 10, 6) - a;
    printf("lower bound : a[%d] = %d \n", idx, a[idx]);
    idx = upper_bound(a, a + 10, 6)- a;
    printf("upper bound : a[%d] = %d \n", idx, a[idx]);
 
}
```

ref : <https://chanhuiseok.github.io/posts/algo-55/>

ref : <https://ansohxxn.github.io/stl/bound/>