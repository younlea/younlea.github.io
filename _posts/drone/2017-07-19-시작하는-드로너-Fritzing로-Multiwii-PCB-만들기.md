---
title: "[시작하는 드로너] Fritzing로 Multiwii PCB 만들기"
excerpt_separator: "<!--more-->"
date: 2017-07-19 00:52:23 +0900
categories:
  - drone
tags:
  - drone
  - 드론

toc : true
toc_sticky : true
---

아두이노로 만든 multiwii 회로를 PCB로 뜰 일이 있어서.. 새로 만들어 봤다. ^^

Fritzing을 사용했고 구매는 해외 직구로 올렸다. (배송비가 PCB가격보다 비싸다 ㅜㅜ)

일다 Fritzing에서 대충 회로를 그린다.

![image](/assets/images/posts/6157820/c0028014_596e2cf84ef1a.jpg)

두번째로는 PCB 라우팅을 자동으로 하도록 아래 버튼을 누른다.

![image](/assets/images/posts/6157820/c0028014_596e2d1714692.jpg)

**20170802 헐.... 프리징이 절 속였네요 ㅜㅜ**

**MPU6050 AD0를 GND에 연결했는데 오토 라우팅할때... 위로 연결되어서..큰 상관은 없지만 ㅜㅜ 에혀**

**확인해보니 MPU6050 라이브러리가 잘못되어 있었네요 떠그럴.. 그나마 1,2,3,4번은 제대로 되어 있어서 일단 패스하고**

**진행하면 될듯합니다. ㅜㅜ**

자동 라우팅이후 제대로 되었는지 확인하는 절차를 아래와 같이 진행한다.

![image](/assets/images/posts/6157820/c0028014_596e2d2e7ce0a.jpg)

DRC를 해보면 어디가 어떻게 안되었다고 나오는데...보면 PCB선들이 겹치는것을 확인해 주는것이다.

이를 잘 옮겨서 위치도 바꾸고해서.. 겹치지 않도록 한다.

해당 모든 연결이 제대로 되었다면 이제 PCB를 만들어도 된다고 말을 해준다 ^^;

그럼 PCB업체에 보내기 위해서 아래와 같은 gerber file을 만든다..

![image](/assets/images/posts/6157820/c0028014_596e2d668bd1f.jpg)

gerber file을 압축해서.. 아래 싸이트에 올리면 짜잔하고 PCB가 오게 된다.

<https://www.seeedstudio.com/>

![image](/assets/images/posts/6157820/c0028014_596e2db007f36.jpg)

가격은.. 대략 20장, 65\*35 2layer에 16$ 그런데 배송비가 18$ ㅜㅜ

자 여기서.. 아래 두곳에서도 구매가 가능한다.

국내 디바이스 마트이다. 여기는 대충 위 PCB가 6만원돈으로 배송 가능하다. (그런데 여기도 해외에서 떠온단다. ㅡ.ㅡ;)

<http://www.devicemart.co.kr/design/index.php?tpl=custom_confirm.htm>

해외 다른 싸이트 하나더.. 여긴 내가 뚫은곳은 아니고 다른 분이 뚫은곳인듯 ^^;

<http://www.skysunpcb.com/>

혹시 몰라서.. 이번에 요청한 multiwii 관련 회로도 파일과 gerber file도 공유한다.

~~`Drone\_multiwii\_v02.zip`~~ *(이글루스 파일 첨부, 서버 종료로 접근 불가)*

~~`multiwii\_v02.zip`~~ *(이글루스 파일 첨부, 서버 종료로 접근 불가)*

이상 허접 PCB제작기 였습니다. ^^;

\