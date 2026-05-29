---
title: "CC3D controller setting guide"
excerpt_separator: "<!--more-->"
date: 2016-11-15 16:16:43 +0900
categories:
  - drone
tags:
  - drone
  - 드론

toc : true
toc_sticky : true
---

CC3D OpenPilot GCS setting guide

굴러다니는 CC3D를 가지고 놀려다 보니... 필자가 업데이트한 아래 링크대로 이제 쓸수 있게된 CC3D를 다시 드론에 붙혀 보려고 합니다.

[https://younlea.github.io/Embedded/CC3D-update-issue/](https://younlea.github.io/Embedded/CC3D-update-issue/)

재료는.. 아래와 같습니다. 최종으로는 시작하는 드로너 책에서 썼던 arduino를 CC3D로 바꾸는걸 연재 하려고 합니다.

[https://younlea.github.io/Drone/시작하는-드로너aliexpress에서-구매하기/](https://younlea.github.io/Drone/시작하는-드로너aliexpress에서-구매하기/)

일단 부품을 아래와 같이 조립을 합니다. (ESC와 모터는 일단 세가닥 서로 연결하고 수신기도 모두 연결합니다.

![image](/assets/images/posts/6066548/c0028014_582ad8f7b907a.png)

일단은 주의 할점은 아래와 같습니다. 순서는 중요하지 않고 방향만이 중요합니다. ^^;

ESC를 CC3D에 연결할 때는 순서는 아래 처럼.. 위에서 부터 차례대로 꽂으면 됩니다.

아래 그림은 오른쪽 부터 1,2,3,4 번이 연결됩니다.

![image](/assets/images/posts/6066548/c0028014_582b02829fa9a.png)

1,2,3,4번은 아래 그림처럼 위치합니다.

![image](/assets/images/posts/6066548/c0028014_582b02fd438f6.png)

수신기를 CC3D에 연결할때는 (바인딩은 Fly sky FS-T6 찾아보시면 금방 아실수 있으니 패스하고)

아래와 같이 signal, VDD, GND 하나를 연결하고 나머지는 왼쪽 제일 처음에가 연결되게 모두 꽂습니다.

마구 꽂아도 됩니다. 나중에 이 부분은 SW적으로 셋팅합니다 ^^;

![image](/assets/images/posts/6066548/c0028014_582b0242e907f.png)

그리고 전원부 연결은 아래와 같이 하면됩니다 ^^;

땜질이 필요한데.. 뭐.. ^^ 제가 구매한 드론 DIY 기체 보드는 12V를 그냥 분기해 주는 역할만 하네요 ^^;

![image](/assets/images/posts/6066548/c0028014_582b031712ba6.png)

자.. 이제 기본 셋팅 준비가 된것 같네요...

이제 OpenPilot GCS를 사용해서 셋팅을 해보도록 하겠습니다.

두둥....

OpenPilot GCS 실행하고 USB를 CC3D에 연결을 합니다.

자. 아래 Vehicle Setup Wizard를 실행합니다.

![image](/assets/images/posts/6066548/c0028014_582b03beb0556.png)

아래 경고를 살포시 무시하고 next를 누릅니다.

![image](/assets/images/posts/6066548/c0028014_582b0409b53a0.png)

오.. firmware update가 나오네요.. 간단히 Upgrade 눌러 줍니다..

![image](/assets/images/posts/6066548/c0028014_582b043f7a25b.png)

업그레이드 완료 되면 Next를 눌러 줍니다.

connection device와 Detected board type 확인하고 next~~~~ 고고고

![image](/assets/images/posts/6066548/c0028014_582b049abfe3d.png)

이번에는 송수신기 방식인데 우리는 PWM을 사용하니 아래 PWM을 선택하고 gogo

![image](/assets/images/posts/6066548/c0028014_582b04d48de2c.png)

우리가 할 기체는 Multirotor니까 Multirotor 정해서 또 고고

![image](/assets/images/posts/6066548/c0028014_582b04fe91f21.png)

쿼드콥터 기체 종류인데 우리는 X 형태이니까.. Quadcopter X를 선택하고 고고

![image](/assets/images/posts/6066548/c0028014_582b05861c967.png)

ESC를 선택하는데 Rapid ESC를 선택하고 고고

![image](/assets/images/posts/6066548/c0028014_582b05ad54a08.png)

이제까지 셋팅한거 보여주고... 다음 고고

![image](/assets/images/posts/6066548/c0028014_582b05d506e21.png)

가속도 자이로 센서를 초기화 해준다고 보면 됩니다. Calculate 클릭후 next~~

![image](/assets/images/posts/6066548/c0028014_582b060402efb.png)

ESC battery 관련 초기화 작업..

![image](/assets/images/posts/6066548/c0028014_582b064427aed.png)

자.. 뭐하지 고민하지 말구... 아래 처럼 체크하고 start....

![image](/assets/images/posts/6066548/c0028014_582b0688e2a3c.png)

이제 베터리 연결하고 소리가 나고.. ...  stop을 누르면 다시 900us까지 가서 소리가 나고 베터리를 빼면 아래 처럼 체크가 사라지고 next가 활성화 됩니다.베터리 켈 쉽죠?? ^^;

![image](/assets/images/posts/6066548/c0028014_582b06ef1cdc9.png)

이제 모터 셋팅하는 부분입니다.~~

![image](/assets/images/posts/6066548/c0028014_582b073f2300a.png)

각 번호의 모터가 보이는 방향으로 잘 돌아가는지 확인하는 단계입니다. (베터리를 다시 연결하세요)

start 버튼을 누르고 아래 처럼 프로그래스 바를 움직이면 모터가 돌기 시작하는데 화살표 방향과 같은지 확인하세요.

![image](/assets/images/posts/6066548/c0028014_582b07c314a2a.png)

그리고 방향이 다르다면 아래 그림에 보이는 모터연결된 선 두개를 서로 교차해서 연결하면 방향이 맞게 될겁니다.

stop을 누르고 선 두개 바꿔서 다시 스타트 하면 확인 가능합니다.

![image](/assets/images/posts/6066548/c0028014_582b07cfe82e3.jpg)

방향이 맞으면  stop을 눌르고 next를 눌러서 다음 모터 체크를 합니다.

네개 모터의 방향을 다 확인했으면 다음으로 고고~~~

이제 기체 선택하는건데.. 대충 250급이니 250급으로 셋팅합니다.

![image](/assets/images/posts/6066548/c0028014_582b085222b23.png)

자.. 이제 셋팅이 끝나가네요. 현재까지 셋팅을 저장합니다.

![image](/assets/images/posts/6066548/c0028014_582b08ab6a9cb.png)

오호 저장이 되고 있네요.

![image](/assets/images/posts/6066548/c0028014_582b08c1414c4.png)

저장이 완료 되면.. 이제 조종기 셋팅하라고 나옵니다.

![image](/assets/images/posts/6066548/c0028014_582b08d98a8d6.png)

-- 1차 셋팅 완료되었고.. 조종기 셋팅은 다음 페이지에서 하도록 하겠습니다.. 헥헥

ref : <http://opwiki.readthedocs.io/en/latest/user_manual/cc3d/cc3d.html>