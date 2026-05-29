---
title: "C# wav file play"
excerpt_separator: "<!--more-->"
date: 2020-09-10 08:43:38 +0900
categories:
  - develop_env
tags:
  - etc

toc : true
toc_sticky : true
---

기본 사용법.

using System.Media;

private SoundPlayer soundPlayer = null;

this.soundPlayer = new SoundPlayer(filePath);

this.soundPlayer.PlayLooping();

this.soundPlayer.Play();

this.soundPlayer.Stop();

// 절대 루트에 있는 파일을 플레이

using System.Media;

SoundPlayer soundPlayer = new SoundPlayer("file 절대 루트 /VLC\_MULTI\_TEST/Crash-Cymbal-1.wav");

soundPlayer.Play();

음원파일을 rsrc에 넣고 로딩하는 방법.

properties.resources.resx에 파일을 넣고 경로를 잘 넣으면 된다. ^^;

using System.Media;

SoundPlayer soundPlayer = new SoundPlayer(VLC\_MULTI\_TEST.Properties.Resources.Crash\_Cymbal\_1);

soundPlayer.Play();

[ref](https://www.it-swarm.dev/ko/c%23/%EB%A6%AC%EC%86%8C%EC%8A%A4%EC%97%90%EC%84%9C-wav-%EC%98%A4%EB%94%94%EC%98%A4-%ED%8C%8C%EC%9D%BC%EC%9D%84-%EC%9E%AC%EC%83%9D%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9E%85%EB%8B%88%EA%B9%8C/970807466/)