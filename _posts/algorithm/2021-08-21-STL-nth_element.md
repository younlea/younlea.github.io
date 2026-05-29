---
title: "STL - nth_element"
excerpt_separator: "<!--more-->"
date: 2021-08-21 12:05:51 +0900
categories:
  - algorithm
tags:
  - AI
  - 머신러닝

toc : true
toc_sticky : true
---

재미있는 녀석인데.. 그러니까. 전체 array에서 n번째까지만 쏘팅한다고 보면 될것 같네요 ^^;

우리가 전체를 소팅하는데 시간이 좀 걸리는데 요거는 딱 n번째까지만 소팅해서 좋은듯요.

nth\_element(vector.begin(), n번째, vector.end(), cmp);

```
// C++ program to demonstrate the use of std::nth_element
#include <iostream>
#include <algorithm>
using namespace std;
int main()
{
    int v[] = { 3, 2, 10, 45, 33, 56, 23, 47 }, i;
 
    // Using std::nth_element with n as 5
    std::nth_element(v, v + 4, v + 8);
 
    // Since, n is 5 so 5th element should be sorted
    for (i = 0; i < 8; ++i) {
        cout << v[i] << " ";
    }
    return 0;
}
 
```

Output:

```
3 2 10 23 33 56 45 47
```

Vector로 구현하게 되면. 중간에 들어가는 부분이 조금 달라진다. ㅡ.ㅡ;

```
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
int main()
{
    vector<int> v = { 3, 2, 10, 45, 33, 56, 23, 47, 60 }, i;
  
    // Using std::nth_element with n as v.size()/2 + 1
    std::nth_element(v.begin(), v.begin() + v.size() / 2, v.end());
  
    cout << "The median of the array is " << v[v.size() / 2];
  
    return 0;
}
```

ref : <https://www.geeksforgeeks.org/stdnth_element-in-cpp/>

<https://neurowhai.tistory.com/274>