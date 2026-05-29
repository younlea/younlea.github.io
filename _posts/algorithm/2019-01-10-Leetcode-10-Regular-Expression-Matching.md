---
title: "Leetcode 10. Regular Expression Matching"
excerpt_separator: "<!--more-->"
date: 2019-01-10 23:09:01 +0900
categories:
  - algorithm
tags:
  - etc

toc : true
toc_sticky : true
---

출처 : <https://leetcode.com/problems/regular-expression-matching/>

문제 : 스트링 매치 확인 문제

입력으로 들어오는 스트링 (S)와 패턴(P)가 있다. 패턴중에 '.'과 '\*'에 대해 처리하시요?

'.' : 어떤 케릭터도 연결이 됩니다.

'\*' : 앞에 오는 케릭터로 매치를 하는데 갯수는 0개에서 무한대까지 대신할수 있습니다.

모든 입력된 스트링에 매치가 되어야 합니다.

S : a~z 만 사용합니다.

P : a~z, '.' '\*' 만 사용됩니다.

흠.. 이렇게 보면 이해가 안가서.. 예를 설명합니다.

패턴으로 들어오는 경우 '.' 은 한개의 어떤 문자도 표현 가능하고.. '\*'는 앞에 있는 문자를 0개에서 무한개까지 커버가 가능하다.

예를 들면.

S : abbb

P : ab\*      -- 패턴이 S를 커버한다.

P : ab..      -- 패턴이 S를 커버한다.

P : ab.      -- 패턴이 S를 커버 못한다. ('.'은 한개만 커버함으로)

p : .b\*      -- 패턴이 S를 커버한다.

P : .\*         -- 패턴이 S를 커버한다. (\* == ... 라고 생각하면 P: '....' 으로 생각할수 있다.

P : c\*a\*b\* -- 패턴이 S를 커버한다. ( c 0개, a 1개, b 3개.. 라고 커버하게 된다. )

원본 문제 (풀이는 원본 문제 아래에 있습니다 ^^)

10. Regular Expression Matching

Given an input string (`s`) and a pattern (`p`), implement regular expression matching with support for `'.'` and `'*'`.

```
'.' Matches any single character.'*' Matches zero or more of the preceding element.
```

The matching should cover the entire input string (not partial).

Note:

- `s` could be empty and contains only lowercase letters `a-z`.
- `p` could be empty and contains only lowercase letters `a-z`, and characters like `.` or `*`.

Example 1:

```
Input:s = "aa"p = "a"Output: falseExplanation: "a" does not match the entire string "aa".
```

Example 2:

```
Input:s = "aa"p = "a*"Output: trueExplanation: '*' means zero or more of the precedeng element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
```

Example 3:

```
Input:s = "ab"p = ".*"Output: trueExplanation: ".*" means "zero or more (*) of any character (.)".
```

Example 4:

```
Input:s = "aab"p = "c*a*b"Output: trueExplanation: c can be repeated 0 times, a can be repeated 1 time. Therefore it matches "aab".
```

Example 5:

```
Input:s = "mississippi"p = "mis*is*p*."Output: false
```

해결은.. 음.. 직접 하다가 삽질해서.. 결국 리커시브로 풀면 답은 나옴.. 그리고 더 최적화를 하면 다이나믹 프로 그램으로 처리 할수 있음.

다이나믹 프로그램은 다음에 업데이트 하도록 하겠습니다.

my solution

bool isMatch(char\* s, char\* p) {

int i =0;

int s\_ln = 0;

int p\_ln = 0;

while(s[i]!='\0')

{

s\_ln++;

i++;

}

i=0;

while(p[i]!='\0')

{

p\_ln++;

i++;

}

if(p\_ln == 0)

{

if(s\_ln == 0)

return 1;

else

return 0;

}

int first\_match = ( s\_ln != 0 && (p[0] == s[0] || p[0] == '.'));

if(p\_ln >=2 && p[1] == '\*')

{

return (isMatch(s, &p[2])|| (first\_match && isMatch(&s[1], p)));

}else{

return (first\_match && isMatch(&s[1], &p[1]));

}

}

\