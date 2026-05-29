---
title: "gbs install error in ubuntu 18.04"
excerpt_separator: "<!--more-->"
date: 2018-12-03 14:13:17 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

After update 18.04, I cannot install gbs package.

In this case, I solved using below metho.d

/etc/apt/sources.list.d$ cat tizen.list

deb **[trusted=yes]** http://download.tizen.org/tools/latest-release/Ubuntu\_16.04/ / # disabled on upgrade to bionic

After modifying the file. you should step below.

sudo apt-get update

sudo apt-get install gbs

\