---
title: "3d robotics - iris+"
excerpt_separator: "<!--more-->"
date: 2016-03-03 02:00:38 +0900
categories:
  - drone
tags:
  - drone
  - 드론

toc : true
toc_sticky : true
---

어쩌다가 지인 분 소개로 iris+ 가진분을 도울 기회가 생겼다.

관련해서 기본 사용 설명서 및  youtube 링크를 올려 놓는다.

[iris+ operation manual](https://3dr.com/wp-content/uploads/2015/02/IRIS-Plus-Operation-Manual-vH-web.pdf)

[Youtube](https://youtu.be/96hxSPYrKYI?list=PLszd_sCs2VsLLqvMM-_ixdkDH5d-k7dqb)

Iris+의 경우 pixhawk를 사용하는걸로 아는데 ground control 관련 동영상도 공유한다.

[ground countrol](https://youtu.be/W2xYFcqSD6E)

[ground controller link](https://3dr.com/download_software/)

[ground controller download link](http://ardupilot.com/wp-content/plugins/download-monitor/download.php?id=91)  (windows용 ground controller)

iris+의 경우 송신기는 Turnigy 9x를 커스터마이징 하고 FR SKY를 사용하고 있다.

FR SKY 1,2번 핀을 아래로 내려야지만 정상적으로 동작하는것을 확인할수 있다. (안그럼 바인딩 부터다시 해야 한다.)

추가로 비행을 위해서는 무조건 GPS가 잘 잡히는 곳에서 해야 하는것으로 보인다.

전면에 LED가 있는데 이 부분이 파란색으로 깜빡여야 한다.

비행은 아래의 순서로 진행해야 함.

1. 송신기를 켠다

2. iris+를 넓은 평지(gps가 안될수 있으므로)에 놓고 베터리 연결한 훈 상단에 있는 빨간색 버튼을 누른다

3. 빨간색 버튼이 깜박이는걸 멈추면 전면에 있는 LED가 파란색으로 깜빡일때까지 대기를 합니다. (GPS 잡을때까지)

4. 파란색으로 깜빡일 경우 조종기 왼쪽 스틱을 아래+ 오른쪽으로 당기면 프로펠러가 돌기 시작합니다.

5. 조종을 시작하고 오른쪽 위에 있는 스틱에 STD mode와 LTR mode, Auto mode가 있습니다.

STD 는 altitude hold mode로 스틱으로 미세 조정을 살짝 해줘야 하는 모드이나 아크로 모드같이 어려운 모드는 아닙니다.

LTR mode의 경우 GPS를 사용하여 스틱을 놓으면 그 자리를 유지하는 모드로 항공 촬영때 쓰면 좋을것 같습니다.

Auto mode의 경우 waypoint비행을 할때 쓰게 되는데 이는 테스트 해보지는 않았습니다. 집근처라 위험해서 ^^;

착륙할때는 왼쪽 상단의 스틱을 사용해서 Land를 누르면 그 자리에 착륙을 하고 오른쪽 위에 있는 스틱을 내리면 Return home이 되어서 그 자리에서 특정고도 까지 올라가서 처음 뜬 위치로 와서 서서히 착륙합니다.

본인은 착륙할때 그냥 throttle을 내려서 착륙했었는데 이상하게 착륙후 넘어지는 현상이 있었는데 저 스틱들을 써서 하니 잘 되더군요

본인의 드론은 아니지만 아는 지인분 덕분에 날려보고 테스트도 해볼수 있어서 좋았습니다. 실제로도 많은 개발자들이 사용하고 있는 드론으로 이스라엘 및 MIT 연구소에서도 사용하고 있어서 한번 날려보고 싶었는데.. 역시 기체 크기가 크기인 만큼 컴퓨터 프로세서를 따로 올릴수 있고 비행 안정성이 확보되어 있어서 개발할때 좋은 기체인듯 합니다. 그러나 본인은 돈이 없어서 ㅜㅜ 패스..

아래는 시험 비행 후 주인에게 돌아 갈때 찍은 영상이다.. 잘 난다.. ^^;\
\