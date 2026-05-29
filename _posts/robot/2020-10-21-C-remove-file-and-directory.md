---
title: "C# remove file and directory."
excerpt_separator: "<!--more-->"
date: 2020-10-21 14:50:18 +0900
categories:
  - robot
tags:
  - robot
  - 로봇

toc : true
toc_sticky : true
---

[c# 파일 삭제](https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/file-system/how-to-copy-delete-and-move-files-and-folders)

```
System.IO.File.Delete(@"C:\Users\Public\DeleteTest\test.txt");
System.IO.Directory.Delete(@"C:\Users\Public\DeleteTest", true);
```

\