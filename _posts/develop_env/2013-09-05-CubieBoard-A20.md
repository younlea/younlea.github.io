---
title: "CubieBoard A20"
excerpt_separator: "<!--more-->"
date: 2013-09-05 22:02:33 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

CubieBoard - <http://cubieboard.org/>

linux install

<http://cubieboard.org/download>

- downloader 받고 설치 (phoenixSuit\_EN.msi)

- linux binary 받기 (<http://cubiebook.org/index.php?title=Cubieboard2/Lubuntu_12.04_Desktop>)

다운로드 모드 들어가는법 : USB otg cable 꽂는 아래 부분 버튼 누르고 USB cable 연결

동영상 가이드 : <http://www.youtube.com/watch?v=LRDseLtEt30>

=> 다운로드 받을때 PC tool 먼저 실행해서 파일까지 선택해 놓고 Cubieboard를 다운로드 모드로 연결.

Format 할꺼냐고 뜨면. YES 눌러서 진행.. (시간이 조금 걸린다.)\

Arch linux

<http://archlinuxarm.org/platforms/armv7/allwinner/cubieboard-2>

nand write하는건 이전 버젼.. 다 무시..

<http://dl.cubieboard.org/software/a20-cubieboard/lubuntu/cb-a20-lubuntu-12.10-v1.06/>

이 버젼을 까니까 된다. ㅡㅜ

11/11) 바쁘다는 핑계로 진행이 더디다.

Linux 버젼깔고나서 루트 권한이 안되서 삽질을 엄청 하고있었는데..

현재 필요한것 두가지는 apt-get으로 sdbd와 gcc compiler 환경 다운받기인데.. 이를 위해서 루트권한이 필요하다.

에고.. 삽질하다 알게된거.

$sudo su - root                << 요렇게 하면 관리자 권한으로 되는구만.

문제는 이후 apt-get 관련해서 또 삽질이 필요하다는거..

안되면 직접 sdbd와 gcc compiler를 arm용으로 빌드를 해야할지도.. 어찌됐던 오늘은 여기까지.

\