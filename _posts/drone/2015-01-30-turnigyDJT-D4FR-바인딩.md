---
title: "turnigy(DJT) D4FR 바인딩."
excerpt_separator: "<!--more-->"
date: 2015-01-30 00:34:08 +0900
categories:
  - drone
tags:
  - drone
  - 드론

toc : true
toc_sticky : true
---

step1...

D4RF 연결을 위한 spec 찾기.. 아웅..이게. 몇년전 모델인지 모르것지만. hobbyking에도 제품 리스트 없어지고 FR-sky.com에도 manual이 없어서 삽질만 했다... google 신도 제대로 물어보지 않으면 안갈켜 준다는걸 뼈저리게 느낌.

일단 설명서 대로 PPM test를 위해 3,4번 쇼트시키고 2번에서 PPM data를 추출해 보자..

![image](/assets/images/posts/5864271/c0028014_54ca5cb3355d6.jpg)

![image](/assets/images/posts/5864271/c0028014_54ca5cb7a9633.jpg)

![image](/assets/images/posts/5864271/c0028014_54ca5cb9c18cf.jpg)

~~`frsky-d4fr.pdf`~~ *(이글루스 파일 첨부, 서버 종료로 접근 불가)*

쩝 송신기랑 페어링을 맞춰야 하는데 ㅡ.ㅡ;

~~`Frsky\_DFT.pdf`~~ *(이글루스 파일 첨부, 서버 종료로 접근 불가)*

자.... 바인딩 시작을 해보자...

1. Turnigy에 연결된 DJT 를 아래와 같이 1,2번을 off로 두고 버튼 눌른상태에서 전원 인가.

-- 그럼 불이 반짝반짝이면서 소리가 나면서 연결 준비

![image](/assets/images/posts/5864271/c0028014_54ca56c57b99d.jpg)

![image](/assets/images/posts/5864271/c0028014_54ca56ce852fd.jpg)

2. D4FR(수신기)을 아래와 같이 3,4번 시그널 핀을 쇼트 시키고 2번+,-에 5V, GND 연결하면 전원인가다.

페어링을 위해서 검은색 버튼을 누르고 전원 인가..

-- 수신기쪽 초록불 빨간불이 켜지고 빨간불 깜빡이면 정상적으로 바인딩 된것임. (초록불 ON, 빨간불 깜빡임 -- bind상태)

![image](/assets/images/posts/5864271/c0028014_54ca56dbb916b.jpg)

![image](/assets/images/posts/5864271/c0028014_54ca56e6ea014.jpg)

3. 둘다 끄고 수신기만 켜면 빨간불이 깜빡이고 송신기를 켜면 수신기의 빨간불이 깜빡이는걸 멈춘다..

연결된거구 ...

![image](/assets/images/posts/5864271/c0028014_54ca573ce828d.jpg)

2번시그널 핀으로 아래와 같은 파형이 나오는걸 볼수 있다...

![image](/assets/images/posts/5864271/c0028014_54ca5743ee4d8.jpg)

![image](/assets/images/posts/5864271/c0028014_54ca574b94c99.png)

이제 바인딩 끝...

ㅋ.. 이거 하는데 얼마나 걸린건지..

이제 아두이노에 Multiwii 포팅해 보자~