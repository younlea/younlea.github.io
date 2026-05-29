---
title: "VMware에 MAC OS(snow leopard)설치하기..."
excerpt_separator: "<!--more-->"
date: 2010-12-02 06:27:34 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

흠 어쩌다 보니 MAC OS에서 툴을 개발해야 하는데.. 집에는 MAC PC가 없어서 윈도우에서 VMWare로 하는 방법을 찾고있다.

역시.. 인터넷의 세계에 용자들이 많은것 같다. ㅋㅋ

<http://tigernet.tistory.com/530> [퍼옴] 파일은 여기가서 받아야 할듯. ㅡ.ㅡ;

﻿오랜만에 지원이나 신변잡기가 아닌 제대로된 포스팅을 해보는 것 같습니다.\

오늘 진행해 볼 내용은 요즘 아이폰,아이패드 등등 애플사의 제품들이 많은 인기를 끌고 있는데요

바로 애플에서 나온 제품들이 사용하는 운영체제를 VMware라는 가상머신상에 설치하는 방법에

대하여 포스팅을 진행해 보겠습니다. 제가 설치한 버전은 snowleopard 일명 눈범이라고 불리는

제품입니다.

[![](http://cfile23.uf.tistory.com/image/201C750B4BE54FC62DC03D)](http://cfile23.uf.tistory.com/original/201C750B4BE54FC62DC03D)

VMware에 설치를 하는 것이니 당연히 VMware는 필요합니다. 저는 workstation 7 버전을 사용했습니다. 그리고 필요한것이 snowleopard 의 설치 디스크입니다. iso이미지로 준비를 해놓는것이

VMware에 설치 할 때 편리합니다.

마지막으로 필요한 것은 VMware상에서 OS X를 지원하지 않기 때문에 이를 지원하게 하기 위해서

OS X의 Kernel 이미지가 필요합니다. 아래에서 다운 받아 압축을 해제하시면 되겠습니다.

VMware 설정

이제 만들어 놓은 가상디스크를 부팅하기에 앞서서 OS X와 관련되 Kernel이미지를 설치해 보겠습니다. 설치파일은 cmd 명령어 스크립트 파일이기 때문에 cmd창을 열어줍니다. 비스타나 윈7을

사용하고 있으면 꼭 관리자 권한으로 실행 시켜 주도록 합니다.

[![](http://cfile6.uf.tistory.com/image/1430C1054BE55458BC7642)](http://cfile6.uf.tistory.com/original/1430C1054BE55458BC7642)

cmd창이 제대로 실행 되었다면 위에서 다운받은 파일의 암축을 해제한 폴더로 이동을 한 후에

해당 setep.cmd install 명령어를 실행하면 VMware상에 OS X의 커널 이미지가 설치되는 것을

볼 수 있습니다. VMware상에서 OS X 를 설치 할 생각정도를 하시는 분들이라면 기본적은 명령어

사용법은 아실거라 믿기 때문에 최종 설치 완료 스샷만 첨부하겠습니다.

[![](http://cfile23.uf.tistory.com/image/193711164BE554FC583C7D)](http://cfile23.uf.tistory.com/original/193711164BE554FC583C7D)

이제 마지막 단계만 남아있습니다. 바로 VMware이미지가 설치된 폴더로 찾아가 기본적인 H/W와

USB 인식등을 하는 설정을 해주어야 합니다. 위의 디스크 공간을 만들때 선택했던 경로로 찾아가

확장자명이 vmx 인 파일을 메모장이나 워드패드로 열어 다음 문구들을 수정하거나 추가해줍니다.

[![](http://cfile6.uf.tistory.com/image/1240BA224BE5562CB2F38C)](http://cfile6.uf.tistory.com/original/1240BA224BE5562CB2F38C)

위의 설정을 모두 마친 후에 가상 디스크를 실행 시키면 설치화면이 동작하는 모습을 볼 수 있습니다. no operation system 등의 문구가 나온다면 위의 설정중 어느 부분이 빠지거나 제대로 진행되지 않았기 때문이니 다시 한번 잘 보고 진행해주시기 바랍니다.