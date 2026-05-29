---
title: "Leetcode 11. Container With Most Water"
excerpt_separator: "<!--more-->"
date: 2019-01-13 22:45:35 +0900
categories:
  - etc
tags:
  - etc

toc : true
toc_sticky : true
---

출처 : <https://leetcode.com/problems/container-with-most-water/>

문제 : 가장 많은 물을 채우면 들어갈수 있는 양은?

난이도 : 중

입력으로 막대기들의 길이와 위치가 주어질때 두개의 막대를 골라서 그 사이에 가장 많이 물을 담을수 있는 방법은?

아래 그림을 보면 아래와 같이 입력이 된다.

막대 길이 ( 1, 8, 6, 2, 5, 4, 8, 3, 7)

막대 위치 ( 1, 2, 3, 4, 5, 6, 7, 8, 9)

여기서 가장 많은 물을 채울수 있는 방법은 아래 빨간 막대 두개를 고르는 것이다. 이때 여기에 들어갈수 있는 물의 양은

7 \* 7 = 49 임을 알수 있다. 이를 해결하는 프로그램을 짜 보도록 하시오~~ 라네요..

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)

문제 원본

Given *n* non-negative integers *a1*, *a2*, ..., *an*, where each represents a point at coordinate (*i*, *ai*). *n*vertical lines are drawn such that the two endpoints of line *i* is at (*i*, *ai*) and (*i*, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container and *n* is at least 2.

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)

The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.

Example:

```
Input: [1,8,6,2,5,4,8,3,7]Output: 49
```

문제 풀이

O(n2)

처음에는 그냥 brute force 로 용감하게.. 다 돌렸습니다.

왼쪽 막대를 기준으로 끝까지 계산해보고 그리고 왼쪽 막대를 한개 증가시켜서 다시 끝까지 계산해 보고...

답은 나오는데 속도는 꼴찌에서 8.41%.. 그러니까 100명중 92등 정도 했네요.. ㅋㅋㅋ

그런데 궁굼한건 100등째에 들려면 어떻게 짜면 되는건지. 흠흠..

int maxArea(int\* height, int heightSize) {

int l = 0;

int r = 0;

int max\_size =0;

int size = 0;

for(l = 0; l < heightSize-1; l++)

{

for(r = l+1; r < heightSize; r++)

{

if(height[l]<= height[r])

size = height[l]\*(r-l);

else

size = height[r]\*(r-l);

if(size > max\_size)

max\_size = size;

}

}

return max\_size;

}

O(n) 방법

좀 생각을 달리해서 왼쪽과 오른쪽에서 줄여들여오면서 짜게 되면 n번만에 찾게 되더군요..

모 이건 100% 안에 드네요.. 흠.. ^^;

#include <stdio.h>

int maxArea(int\* height, int heightSize) {

int l = 0;

int r = heightSize-1;

int max\_size =0;

int size = 0;

while(l<r)

{

if(height[l]<=height[r])

{

size = height[l]\*(r-l);

l++;

}else

{

size = height[r]\*(r-l);

r--;

}

if(size > max\_size)

max\_size = size;

}

return max\_size;

}

int main(int argc, char \*argv[])

{

int height[10]={1,8,6,2,5,4,8,3,7};

printf("\n max\_area %d\n",maxArea(height, 10));

return 0;

}