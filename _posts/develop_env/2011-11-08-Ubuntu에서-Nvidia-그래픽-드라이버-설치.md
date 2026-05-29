---
title: "Ubuntu에서 Nvidia 그래픽 드라이버 설치."
excerpt_separator: "<!--more-->"
date: 2011-11-08 13:33:39 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

<http://www.geforce.com/drivers> 에 들어가서 운영체제만 선택하고 다운로드를 받음.

(흠.. firefox로 안받아져서 google chrome로 받았다)

나머지는 아래 블로그에서 링크따왔습니다 ^^;

[<http://blog.simplism.kr/?p=258>]

## 1. 드라이버 다운로드

<http://www.geforce.com/drivers>

[![](http://simplism.kr/wordpress/wp-content/uploads/2009/12/nvidia.png "nvidia")](http://simplism.kr/wordpress/wp-content/uploads/2009/12/nvidia.png)

위 처럼 NVIDIA-Linux-x86……pkg1.run 이란 파일이 생겼으면 이제 다음 단계로 넘어간다.

### 2. 콘솔 모드로 나오기

거의 모든 리눅스 배포판들은 Ctrl + Alt + F1 ~ F6까지 tty(TeleTypewriter)라는 일종의 터미널이 있습니다. Ctrl + Alt + F7 을 누르면 다시 X윈도우로 넘어오게 되구요… (Suse, CentOS, Fedora, Ubuntu, Mint를 잠깐씩 써봤는 데, 거의 다 똑같았던 것으로 기억함..)

드라이버를 설치하기 위해서 X윈도우(또는 X서버)를 꺼야합니다. 수세나 센트OS에서는/sbin/init 3을 이용하여 강제로 커널의 런레벨을 조정해서 X서버를 껐었는 데, 우분투는 희안하게도…

[![](http://simplism.kr/wordpress/wp-content/uploads/2009/12/nvidia2.png "nvidia2")](http://simplism.kr/wordpress/wp-content/uploads/2009/12/nvidia2.png)

경고를 띄우는 것도 아니고… 그냥 먹어버립니다.(아무런 동작을 취하지 않습니다.)\
그래서 관련 문서를 구글링해서 찾아보니… 스크립트를 이용해서 GDM(즉, 그놈데스크탑환경)을 끌 수 있더군요…

이런 것들을 겪을 때마다 리눅스가 대중화가 힘든 점들 중에 하나가 이런 것이라고 생각합니다. 배포판마다 공통의 Trouble Shooting(문제 해결)이 없다는 것이지요… (어느정도 일치하는 면도 있지만, 세부적으로는 굉장히 많은 차이들을 가지고 있는 경우도 있습니다. 각기 다른 패키지 관리 방식이라던지… 아, 잠깐 딴 길로 셌군요..ㅎㅎ)

**자자, 일단 Ctrl + Alt + F1을 눌러서 TTY1로 갑니다.**

그러면, 검은 화면에 저의 경우는 아래처럼 나옵니다.

> Ubuntu 9.10 simplism-desktop tty1
>
> simplism-desktop login : simplism\
> password :
>
> …. (중략 )….
>
> simplism@simplism-desktop : ~$

이 처럼 tty 모드로 작업을 하겠습니다. 다시 말씀드리지만, 이 상태에서 **그래픽모드**로 전환하려면, **Ctrl + Alt + F7**을 눌러주시면 됩니다.

### **3. GDM을 종료시킵니다.**

> $ sudo /etc/init.d/gdm stop

을 하면, 종료합니다. 다시 켤 때는, stop을 start로 바꿔주면 다시 켜집니다. /etc/init.d/에 있는 gdm이라는 스크립트에 stop이라는 매개변수를 넣어주어서 gdm을 종료하는 것입니다. 다시 켤때는 매개변수를 start로 바꿔주니까 start하는 스크립트로 동작하겠지요.

### 4. 드라이버 설치 파일을 실행시킵니다.

이제는 조금 전에 홈폴더에 저장했던 NVI….pkg1.run파일을 실행할 차례입니다. 원래 CentOS에서 이 파일을 실행할 때, 권한 설정이 필요했던 것 같은데… 이상하게 그냥 해도 실행이 되더군요.\
일단 NVI…pkg1.run파일을 실행하기 위해서는 루트(root)권한이 필요하고, GDM이 종료되어 있는 상태에서 실행을 해야합니다. 위에 이미 GDM을 종료시켜 두었기에, 아래처럼 실행하면 됩니다.

> $ sudo sh ./NVI…pkg1.run

위 처럼 sudo를 이용해서 root권한으로 동작하도록 하며, tty 로그인 시 기본적인 위치는 해당 계정의 홈폴더이므로, 조금 전에 저장했던 파일과 동일한 위치이기에 경로는 ./(현재 위치)입니다.\
파일명은 굉장히 긴 편이므로… NVI까지만 입력하고서 탭을 눌러주시면 자동완성을 시켜줍니다. (NVI로 시작하는 다른 파일이 없다면요..ㅎ)

이제 비밀번호를 입력하고, 엔터 몇 번만 쳐주면 드라이버를 빌드하고 시스템에 적용을 합니다. 그리고 재부팅을 하면 이제 정상적으로 엔비디아 드라이버로 우분투를 사용할 수 있는 것입니다.

^^잘되네요.