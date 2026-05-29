---
title: "linux folder size check and find leaks file"
excerpt_separator: "<!--more-->"
date: 2016-11-23 10:55:56 +0900
categories:
  - develop_env
tags:
  - algorithm
  - 알고리즘

toc : true
toc_sticky : true
---

du -sh \*   << you can find top down folder.

If you want quickly find leaks file. you can your below method.

/#du -ck | sort -n