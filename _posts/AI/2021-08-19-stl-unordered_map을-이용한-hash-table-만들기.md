---
title: "stl - unordered_map을 이용한 hash table 만들기 ^^;"
excerpt_separator: "<!--more-->"
date: 2021-08-19 14:56:20 +0900
categories:
  - AI
tags:
  - AI
  - 머신러닝

toc : true
toc_sticky : true
---

움.. hash를 C 로 짜면.. char을 long long으로 만들어서 key값을 만드는데..

그렇게 안하고 stl에 있는 unordered\_map을 쓰면 아래와 같이 쓸수 있다.

단.. 웃긴건 myhash["str"]; 만 입력해도 str 에 대한 값이 입력이 되어서..

myhash.count("str")로 있는지 확인하고 있으면 myhash["str"]을 읽어오는 방식을 써야 한다. ^^;

(myhash.count()의 리턴값이 있으면 1, 없으면 0이다 ^^)

hash data를 지우는건 myhash.erase(string) 이다.

결국 string 을 num에 메칭하는 간단한 방법이다.

이걸 쓰면 좋은점은  hash map을 직접 만들때면 지우고 다시 넣을때 좀 까다로웠는데.. 그럴 필요가 없다 정도 ^^;

첨언하면. 움.. c로 구현할때는 충돌나는 이슈로 지우는게 좀 쉽지 않아서 flag를 썼는데 이건 그렇지 않아도 되는데.

대신 지워지는 만큼 배열 size를 크게 잡아야 한다

순차적으로 string에 대한 대응 값을 증가 시키기 때문이다.

코드 구현의 편의를 위해서 만들었다 정도?

선언 unordered\_map <string, int> m;

추가 m[str] = val;

값 사용 m[str];   //유의사항 처음 str을 사용하는거면 0이 입력될수 있다. 확인후 없을 때만 읽어라.

확인 m.count(str); //있으면 1, 없으면 0

지우기 m.erase(str);

전체 지우기 m.clear();

```
#include <unordered_map>
using namespace std;
 
unordered_map<string, int> myhash;
 
int gCnt =0;
 
int getHashKey(const char *str)
{
    if(myhash.count(str))
        return myhash[str];
    else
        return -1;
}
 
void addHash(const char *str)
{
    if(myhahs.count(str) == 0)
    myhash[str] = gCnt++;
}
 
void delHash(const char *str)
{
    if(myhash.count(str))
        myhash.erase(str);
}
 
void checkData(const char * str)
{
    int ret = getHashKey(str);
    
    if(ret != -1)
        printf("%s - %d \n", str, ret);
 
}
 
void main()
{
    gCnt = 0;
    
        addHash("aaa");
        addHash("bbb");
        addHash("aaaaaaa");
        
        checkData("aaa");    
        delHash("aaa");
        checkData("aaa);
        myhash.clear();
}
```

간단한 테스트 예제

```
#include <algorithm>
#include <vector>
#include <stdio.h>
using namespace std;
 
 
void main()
{
    unordered_map<string, int> um;
 
    printf("um.count(\"string\") : %d \n ", um.count("string"));
    um["string"] = 100;
    printf("after set value and um.count(\"string\") :%d \n ", um.count("string"));
    printf("um[\"string\"] : %d \n", um["string"]);
    um.erase("string");
    printf("um.count(\"string\") after um.erase(\"string\") :%d \n ", um.count("string"));
}
```

\