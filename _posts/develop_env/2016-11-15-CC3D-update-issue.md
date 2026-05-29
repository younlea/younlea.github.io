---
title: "CC3D update issue\"
excerpt_separator: "<!--more-->"
date: 2016-11-15 00:24:37 +0900
categories:
  - develop_env
tags:
  - develop_env
  - embedded
  - 임베디드

toc : true
toc_sticky : true
---

CC3D 일년전에 산 버젼이 openpilot GCS 15.02에 연결이 안되고 펌웨어가 업데이트가 안된다.

아래 처럼 timed out while waiting for a board..... 라고 나오면서 안된다 ㅜㅜ

![image](/assets/images/posts/6066347/c0028014_582a7566ec390.png)

firmware version은  ... . 아래와 같이.. .나오지만..

![image](/assets/images/posts/6066347/c0028014_582a75af23bd5.png)

장치관리자를 보니... 포트가 잡혀있지 않다..

![image](/assets/images/posts/6066347/c0028014_582a75ccb19f9.png)

정상적인 경우.. 아래와 같이 포트가 나와야 한다.

![image](/assets/images/posts/6066347/c0028014_582a760289588.png)

이슈를 찾다보니.... 펌웨어 문제인것 같아서.. 해결책윽 찾다보니..  openpilot GCS에서 펌웨어 업데이트 하는 방법이 있다.

도전....

Firmware 텝에 와서 rescue 버튼을 누른다.

![image](/assets/images/posts/6066347/c0028014_5829d6c471476.png)

아래와 같이 time out 나기 전에 cc3d를 연결한다.

![image](/assets/images/posts/6066347/c0028014_5829d6e9da1f3.png)

연결후 보드가 아래와 같이 뜨면 바이너리를 open해서 다운로드 하면 된다.

![image](/assets/images/posts/6066347/c0028014_5829d6ff4c849.png)

부트로더는 아래 싸이트에서 받으면 될것 같았으나.. 안된다. .ㅜㅜ

<http://opwiki.readthedocs.io/en/latest/appendices/bootloader.html>

해결 방법을 찾다 보니... 정상적으로 되는 openpilot cc3d보드에서 바이너리를 빼서 그걸 쓰면 해결되더라는..

1. Rescue를 누르고 정상적으로 되는 보드 연결후 Retrieve 버튼 클릭.

2. 해당 보드에서 바이너리를 읽어서 저장한다.

필자가 직접 읽어서 저장한 파일이다.. 이 파일을 다운로드 하면 되긴 하다 ^^;

~~`test\_cc3d\_pilot.zip`~~ *(이글루스 파일 첨부, 서버 종료로 접근 불가)*

파일을 open 하면 아래 처럼 다운로드를 하기 위해  I know what I'm doing! 을 체크해야 Flash 가 가능하다..

이제 다운로드를 한다~~~ 앗싸..

![image](/assets/images/posts/6066347/c0028014_582a70f6eecc9.png)

다운로드 완료후 보드 USB 를 제거하고 Open pilot GCS 를 재 시작한후에 firmware 누르고 USB 를 연결하면...

짜잔.. 장치 관리자에 포트도 잡히고.. 링크가 정상적으로 연결이 된다. ^^;

그런데 여기가 끝이 아니다.. 왜냐..  펌웨어 버젼이 제대로 나오지 않는다 ㅜㅜ

![image](/assets/images/posts/6066347/c0028014_582a729a8ac93.png)

다시 한번.. 바이너리를 받아야 하는데 정식 버젼을 받으면 해결이 된다.

자 그럼..  GCS 처음 실행해서  아래 Vehicle Setup  Wizard 를 클릭하고

![image](/assets/images/posts/6066347/c0028014_582a7370402ec.png)

firmware를 업데이트 한다.

![image](/assets/images/posts/6066347/c0028014_582a72fd80028.png)

업데이트 완료후 firmware를 확인하면... 짜잔.. 정식 버젼 업데이트가 되서.. 모두 정상으로 나오는걸 볼수 있습니다. ^^;

![image](/assets/images/posts/6066347/c0028014_582a7325b5dca.png)

PS : 오래된 버젼의 문제인지는 모르겠지만.. 혼자 cleanflight가 다운되어 있는건가?? 이것저것 고민하면서 삽질을 ㅜㅜ

참고  site

<https://librepilot.atlassian.net/wiki/display/LPDOC/Downloads>

<https://librepilot.atlassian.net/wiki/display/LPDOC/CC+Hardware+Configuration>

<http://opwiki.readthedocs.io/en/latest/user_manual/cc3d/cc3d.html>

<http://opwiki.readthedocs.io/en/latest/appendices/bootloader.html>