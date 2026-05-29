---
title: "[Ubuntu]notebook  모니터 닫았을때 꺼지지 않게 하기."
excerpt_separator: "<!--more-->"
date: 2019-11-13 12:28:58 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

제목대로 notebook을 켜놓구 shell을 통해서 개발할때 노트북을 덮으면 자꾸 꺼져서 이를 해결하는 방법을 찾아 봤습니다.

1. 파일 수정.

**sudo vim /etc/systemd/logind.conf**

아래 빨간부분 주석을 풀고 lock 으로 하시면 됩니다.

14 [Login]

15 #NAutoVTs=6

16 #ReserveVT=6

17 #KillUserProcesses=no

18 #KillOnlyUsers=

19 #KillExcludeUsers=root

20 #InhibitDelayMaxSec=5

21 #HandlePowerKey=poweroff

22 #HandleSuspendKey=suspend

23 #HandleHibernateKey=hibernate

**24 HandleLidSwitch=lock**

25 #HandleLidSwitchDocked=ignore

26 #PowerKeyIgnoreInhibited=no

27 #SuspendKeyIgnoreInhibited=no

28 #HibernateKeyIgnoreInhibited=no

29 #LidSwitchIgnoreInhibited=yes

30 #HoldoffTimeoutSec=30s

31 #IdleAction=ignore

32 #IdleActionSec=30min

33 #RuntimeDirectorySize=10%

34 #RemoveIPC=yes

35 #InhibitorsMax=8192

36 #SessionsMax=8192

37 #UserTasksMax=33%

2. 재시작~~

**systemctl restart systemd-logind.service**

자 이제 노트북 뚜껑 덮고 하시면 됩니다~~

~