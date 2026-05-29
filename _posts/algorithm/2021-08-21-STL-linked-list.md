---
title: "STL - linked list"
excerpt_separator: "<!--more-->"
date: 2021-08-21 11:39:10 +0900
categories:
  - algorithm
tags:
  - algorithm
  - 알고리즘

toc : true
toc_sticky : true
---

움 linked list를 C에서 직접 짜서 쓰긴 하는데...

C++에 있는 STL library를 쓰는게 안전해서 정리해 봅니다요.

기본적을 double linked list를 구성되어있네요.

일단 만들때는. list를 include 해야하고..

안에 값을 보기 위해서는 C와 마찬가지로 순회를 해서 봐야 합니다. 이때 사용하는게 iterator이고요..

list의 좋은건..

push\_front(), push\_back()으로 앞과 뒤에서 넣을수도 있고

pop\_front(), pop\_back으로 앞과 뒤에서 뺄수도 있습니다.

remove, remove\_if()도 있고.. 요건 원하는 것만 쏙 지울수도 있고 조건에 맞는것만 지울수도 있습니다.

insert랑 erase도 있긴한데.. 훔 잘 안쓰는것 같기도 하구 ㅡ.ㅡ;

제일 앞과 뒤의 값을 볼때는 list.front(), list.back()을 쓰면 되네요.

그리고 reverse와 sort(), merge(소팅된 두개 list)가 있어서 사용이 편하긴 합니다.

마지막으로 splice()라고 있는데 요게 쓰기 참 좋은것 같네요.

두개의 linked list A,B가 있으면 A에 B를 붙이는 겁니다. 사이에 끼워 넣는데 ^^; 이걸 직접 짜면 머리가 ㅜㅜ

<https://www.geeksforgeeks.org/list-splice-function-in-c-stl/>

```
#include <iostream>
#include <list>
#include <iterator>
using namespace std;
 
//function for printing the elements in a list
void showlist(list <int> g)
{
    list <int> :: iterator it;
    for(it = g.begin(); it != g.end(); ++it)
        cout << '\t' << *it;
    cout << '\n';
}
 
int main()
{
 
    list <int> gqlist1, gqlist2;
 
 
    for (int i = 0; i < 10; ++i)
    {
        gqlist1.push_back(i * 2);
        gqlist2.push_front(i * 3);
    }
    cout << "\nList 1 (gqlist1) is : ";
    showlist(gqlist1);
 
    cout << "\nList 2 (gqlist2) is : ";
    showlist(gqlist2);
 
    cout << "\ngqlist1.front() : " << gqlist1.front();
    cout << "\ngqlist1.back() : " << gqlist1.back();
 
    cout << "\ngqlist1.pop_front() : ";
    gqlist1.pop_front();
    showlist(gqlist1);
 
    cout << "\ngqlist2.pop_back() : ";
    gqlist2.pop_back();
    showlist(gqlist2);
 
    cout << "\ngqlist1.reverse() : ";
    gqlist1.reverse();
    showlist(gqlist1);
 
    cout << "\ngqlist2.sort(): ";
    gqlist2.sort();
    showlist(gqlist2);
 
    return 0;
 
}
 
```

ref : <https://www.geeksforgeeks.org/list-cpp-stl/>

<https://sanghyu.tistory.com/84>

<https://blockdmask.tistory.com/76>