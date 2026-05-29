---
title: "C# open windows explorer"
excerpt_separator: "<!--more-->"
date: 2020-10-21 14:31:46 +0900
categories:
  - etc
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

open d:\

```
using System.Diagnostics;
 
string filepath = "D:\\";
Process.Start(filepath);
```

C#에서 윈도우 탐색기를 열때 사용합니다. ^^;