---
title: "ubuntu + win7 + winxp multi booting.. (멀티부팅)"
excerpt_separator: "<!--more-->"
date: 2011-09-23 16:49:40 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

인터넷에 용자들은 참 많습니다 ~~

win7에 ubuntu 10.04를 멀티부팅하려다가 안되서 찾다가 아래분의 포스팅을 보고 해결했다는 ^^;

널리 사용하시라고 공유합니다.

<http://noneway.tistory.com/49>

한대의 컴퓨러에 리눅스와 MS윈도우를 동시에 설치하고 자 하면 분할된디스크([파티션](http://kr.dictionary.search.yahoo.com/search/dictionary?p=%C6%C4%C6%BC%BC%C7&ret=1&fr=kr-search_top))가최소 2개이상은 있어야 한다.

현재 윈도우가단일파티션으로 깔려있다면 윈도우를 다시설치하며 2개이상의 파티션을 해줘야 한다. 다시깔지 않고파티셔닝 할 수 이있는 방법도 있는데, [비스타에서는 자체적으로 가능](http://blog.naver.com/ggutle/150025757192)하지만,XP는 [파티션매직](http://kr.search.yahoo.com/search?fr=kr-front_sprit&KEY=&p=%ED%8C%8C%ED%8B%B0%EC%85%98%EB%A7%A4%EC%A7%81)같은 상용프로그램을 이용해야 한다.

XP에서 파티션 상태를 보려면 시작 > 보조프로그램 > 윈도우탐색기(또는 탐색기>내컴퓨터에서 오른클릭>관리>디스크관리)를 실행하여,플로피(A: B:)와 ODD장치(CD, DVD, RW등의 드라이브)및이동식디스크(플레쉬메모리)를 제외한 나머지가하드디스크의 파티션 갯수이다. 그림의 컴퓨러는 C:와D:두개의 파티션이 존재함을 확인할 수 있다.

**윈도우설치 시 파티션 마련하기**

윈도우를 깔때 윈도우파티션(C:)와 리눅스와 공유할 개인저장소(D:)와 리눅스를 설치할 비할당공간을 아래의 설명을 참조하여준비한다.(윈도우설치방법은 인터넷에 널렸다.)\

리눅스를 먼저깔고 윈도우를 깔면\
윈도우가 리눅스를 인식하지 못해 멀티부팅옵션이 생성되지 않아 부팅불가가 된다.\
시장지배적위치에 있는 MS로선 궂이 리눅스를 지원할 필요성을 느끼지 않을 것이므로 가슴이 넓은 우리가 이해 해줄 수 밖에 없을것 같다.\

리눅스를 설치할 영역은, 비할당으로 남겨 놓는 게, 디스크분할 시 알아보기 편하다.

아래 그림은 30Gb 하드디스크에\

c: ▶ 윈도우[10489Mb] \
d: ▶ 개인저장공간[10923Mb]\
우분투를 설치할 ▶ 비할당공간[8592Mb]\

의 디스크준비 상황을 갈무리한 모습이다.\
c:와 d:는 fat든 ntfs든 상관이 없으나, 대용량 디스크라면 ntfs를 추천한다.

[![](http://cfs5.tistory.com/upload_control/download.blog?fhandle=YmxvZzE2NDM2MEBmczUudGlzdG9yeS5jb206L2F0dGFjaC8wLzE3MDAwMDAwMDA1MC5wbmc%3D)](http://cfs5.tistory.com/upload_control/download.blog?fhandle=YmxvZzE2NDM2MEBmczUudGlzdG9yeS5jb206L2F0dGFjaC8wLzE3MDAwMDAwMDA1MC5wbmc%3D)

디스크분할 설정창에서 "수동으로"를 선택했을때 만나는 대화상자

[![](http://cfs5.tistory.com/upload_control/download.blog?fhandle=YmxvZzE2NDM2MEBmczUudGlzdG9yeS5jb206L2F0dGFjaC8wLzE3MDAwMDAwMDA1MS5wbmc%3D)](http://cfs5.tistory.com/upload_control/download.blog?fhandle=YmxvZzE2NDM2MEBmczUudGlzdG9yeS5jb206L2F0dGFjaC8wLzE3MDAwMDAwMDA1MS5wbmc%3D)

"Free space"를 선택하고 "New partition"단추를 눌러서 "새파티션 만들기" 창을 만나보도록 하자\
리눅스에서 사용하려고 준비한 총용량이 "8Gb" 가량 됨을 볼 수 있다.

[![](http://cfs6.tistory.com/upload_control/download.blog?fhandle=YmxvZzE2NDM2MEBmczYudGlzdG9yeS5jb206L2F0dGFjaC8wLzE3MDAwMDAwMDA0MS5wbmc%3D)](http://cfs6.tistory.com/upload_control/download.blog?fhandle=YmxvZzE2NDM2MEBmczYudGlzdG9yeS5jb206L2F0dGFjaC8wLzE3MDAwMDAwMDA0MS5wbmc%3D)

swap에 할당할 1Gb의 용량을 뺀 나머지 용량을 입력하고\
용도 "ext3"\
마운트 위치 "/"로 지정하고 "OK"를 눌러 진행하면 된다.

[![](http://cfs6.tistory.com/upload_control/download.blog?fhandle=YmxvZzE2NDM2MEBmczYudGlzdG9yeS5jb206L2F0dGFjaC8wLzE3MDAwMDAwMDA0Mi5qcGc%3D)](http://cfs6.tistory.com/upload_control/download.blog?fhandle=YmxvZzE2NDM2MEBmczYudGlzdG9yeS5jb206L2F0dGFjaC8wLzE3MDAwMDAwMDA0Mi5qcGc%3D)

※ 참고 : /는 위에도 말했듯이 리눅스가 깔리는 공간이다.\
우분투 설치 후 용량이 2.3Gb에 불과함을 염두해 두고 디스크분할를 진행하자.\

[![](http://cfs4.tistory.com/upload_control/download.blog?fhandle=YmxvZzE2NDM2MEBmczQudGlzdG9yeS5jb206L2F0dGFjaC8wLzE3MDAwMDAwMDAzMS5wbmc%3D)](http://cfs4.tistory.com/upload_control/download.blog?fhandle=YmxvZzE2NDM2MEBmczQudGlzdG9yeS5jb206L2F0dGFjaC8wLzE3MDAwMDAwMDAzMS5wbmc%3D)

리눅스를 설치할 파티션[ext3, /]이 성공적으로 생성됐음을 볼수있다.

다음은 "swap"영역을 할당할 것이다. ("/"와 "swap"은 필수 파티션이다)

[![](http://cfs4.tistory.com/upload_control/download.blog?fhandle=YmxvZzE2NDM2MEBmczQudGlzdG9yeS5jb206L2F0dGFjaC8wLzE3MDAwMDAwMDAzMi5wbmc%3D)](http://cfs4.tistory.com/upload_control/download.blog?fhandle=YmxvZzE2NDM2MEBmczQudGlzdG9yeS5jb206L2F0dGFjaC8wLzE3MDAwMDAwMDAzMi5wbmc%3D)

다시 "Free space"를 선택하고 "New partition"단추를 눌러서 "새파티션 만들기" 창을 만나자

위와 같이 용도를 "swap"으로 설정하고 "OK"를 눌러준다.

[![](http://cfs5.tistory.com/upload_control/download.blog?fhandle=YmxvZzE2NDM2MEBmczUudGlzdG9yeS5jb206L2F0dGFjaC8wLzE3MDAwMDAwMDA1Mi5wbmc%3D)](http://cfs5.tistory.com/upload_control/download.blog?fhandle=YmxvZzE2NDM2MEBmczUudGlzdG9yeS5jb206L2F0dGFjaC8wLzE3MDAwMDAwMDA1Mi5wbmc%3D)

"swap"파티션도 성공적으로 작성됐다.\

MS윈도우로 말하면 가상메모리(=스왑)역할을 한다. 결국 같은 말이돼버렸다 ㅎㅎ\
그림과 같이 "ext3" "/" 영역에 v 표를 찍어주고 "앞으로"를 눌러주자.

"앞으로"를 눌러 넘어간 화면에 "문서와 설정을 ~어쩌구" 하는 창이 나올것이다.\
아무것도 건딜지말고 "앞으로"를 눌러주자

[![](http://cfs4.tistory.com/upload_control/download.blog?fhandle=YmxvZzE2NDM2MEBmczQudGlzdG9yeS5jb206L2F0dGFjaC8wLzE3MDAwMDAwMDAzMy5wbmc%3D)](http://cfs4.tistory.com/upload_control/download.blog?fhandle=YmxvZzE2NDM2MEBmczQudGlzdG9yeS5jb206L2F0dGFjaC8wLzE3MDAwMDAwMDAzMy5wbmc%3D)

설치된 HDD가 2개 이상이고,\
부트로더(grub)이 설치되는 HDD를 지정하고 싶은 사용자는 그림과 같이 변경하면 된다. \
두번째 HDD에 설치하려면 (hd1)로 변경하면 된다.

이제 "install"을 눌러, 설치하는 모습을 구경만 하면 끝이다. 너무 쉽지 않은가?

※참고 :\

파티션중에도\
"플레이스->컴퓨터" 실행하여\
드라이브 상태를 확인하고 파티션 한다면, 중요자료 날리고 후회하는 일이 없을듯 하다