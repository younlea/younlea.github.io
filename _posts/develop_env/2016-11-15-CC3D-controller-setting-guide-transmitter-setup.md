---
title: "CC3D controller setting guide (transmitter setup)"
excerpt_separator: "<!--more-->"
date: 2016-11-15 22:45:59 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

이전 장에서 CC3D setting을 하고 나서.. 아래와 같이 Transmitter setting을 연속해서 계속 해도 되고...

![image](/assets/images/posts/6066688/c0028014_582b094f91469.png)

아니면.. OpenPilot 메뉴에서 선택해도 됩니다.

아래에 있는 configuration -> 왼쪽에 Input -> 가운데 start transmitter setup wizard를 눌러도 됩니다.

![image](/assets/images/posts/6066688/c0028014_582b09ab80160.png)

자.. 셋팅에 앞서..FLY SKY FS-T6에서 아래 키를 셋팅을 하기 위해서는 조종기 셋팅을 먼저 해야 합니다.

![image](/assets/images/posts/6066688/c0028014_582b0b40d0260.png)

일단 조종기 셋팅을 하도록 하겠습니다.

왼쪽 손가락 있는 부분을 클릭하면 아래와 같이 선택지가 나옵니다.

![image](/assets/images/posts/6066688/c0028014_582b0b67a6440.jpg)

자.. 오른쪽으로 굴리면.. setup으로 가게 됩니다.

![image](/assets/images/posts/6066688/c0028014_582b0b89d0d42.jpg)클릭을 하면...다음으로 들어가고 다시 굴려서 Aux. channels를 선택하고 눌러줍니다.

![image](/assets/images/posts/6066688/c0028014_582b0b955eb2f.jpg)

여기서.. 또 굴리면 Source 뒤에 있는 값이 바뀌게됩니다. 우리가 쓸건. SWC 키를 channel 5로  쓸꺼니까 아래와 같이 셋팅하고 오른쪽의 OK를 눌러서 셋팅을 끝냅니다.

![image](/assets/images/posts/6066688/c0028014_582b0bbd3a755.jpg)

자.. 이제.. 조종기 셋팅이 끝났습니다. (아.. 이거 Mode1이랑 Mode2 셋팅도 가능한데 이건 다음 시간에 하도록 하겠습니다.)

어찌됐던.. 이제.. OpenPilot에서 조종기를 셋팅해 보도록 하겠습니다.

" start transmitter setup wizard"를 눌르면 아래와 같은 팝업이 뜰겁니다.

그러니까..셋팅하다가 모터가 돌면 안되니까... 무조건 모터 안돌게 하겠다 뭐.. 그런 말입니다. 일당 오케이 하고 고고

![image](/assets/images/posts/6066688/c0028014_582b0c5ad19a1.png)

자.. 이제 본격적으로 조종기 셋팅에 들어가 봅시다. next gogo

![image](/assets/images/posts/6066688/c0028014_582b0f04c2a22.png)

우리가 사용할건 quad 이니 acro로 셋팅하고  다음..

![image](/assets/images/posts/6066688/c0028014_582b0f1983dc7.png)

현재 구매한 조종기가 Mode2라 Mode 2로 선택하고...(다음 시간에는 Mode1으로 바꾸는걸 해보겠습니다.)

![image](/assets/images/posts/6066688/c0028014_582b0f8b560e9.png)

이제.. 각 조종기 스틱을 셋팅하는 시간입니다. 기억하실지 모르겠지만 수신기 선을 맘대로 연결하라고 했던게 Multiwii와는 다르게 어떤신호가 들어오던.. 조종기와 CC3D쪽 SW가 서로 메칭을 하게 할수 있어서 입니다. 겁니 편해졌죠 ^^;

자  throottle 움직여 봅니다.

![image](/assets/images/posts/6066688/c0028014_582b0f36e8b40.png)

Roll stick 움직입니다.

![image](/assets/images/posts/6066688/c0028014_582b0fe250c74.png)

pitch stick 움직입니다.

![image](/assets/images/posts/6066688/c0028014_582b0ff104fdd.png)

yaw stick 움직입니다.

![image](/assets/images/posts/6066688/c0028014_582b101451422.png)

자.. 아까 조종기에서 셋팅했던 Mode switch를 아주 빨리 움직여 봅니다 ^^;

![image](/assets/images/posts/6066688/c0028014_582b100af28c9.png)

자 이제 셋팅이 되었으니 다 막 움직여 봅니다.. ^^; 그리고 멈추면 스틱들이 멈춰있는걸 알수있을겁니다.

최대 최소값을 메칭하기 위한 작업입니다 ^^; 그러니 전체를 다 돌려주시면 됩니다 ^^;

![image](/assets/images/posts/6066688/c0028014_582b102e980fc.png)

이제.. 막 움직였으니.. 정 중앙에 놓구.. 다음으로 넘어갑니다.

![image](/assets/images/posts/6066688/c0028014_582b105bf2191.png)

앗. 다시 한번 막 움직여야 하네요

![image](/assets/images/posts/6066688/c0028014_582b10a69aad0.png)

자.. 이제.. 다음은.. 혹시 스틱 방향과 다르게 움직이지 않는지 확인하는 시간입니다.

저는 pitch가 반대로 움직여서 아래와 같이  pitch에 셋팅했습니다. 이제 반대로 움직이는건 없어 집니다~~

방향을 맞추는 단계 입니다.

![image](/assets/images/posts/6066688/c0028014_582b10b2eb9aa.png)

자 이제 다 됐다네요

![image](/assets/images/posts/6066688/c0028014_582b10eed53b7.png)

그럼 이제 arming 하는 방법을 셋팅합니다.

![image](/assets/images/posts/6066688/c0028014_582b110127151.png)

저는 throttle min 값에  yaw right로 5초 동안 하면 arming이 걸리게 했습니다.

save를 하고... 조종기를 아래와 같이 하면 arming 되는걸 알수 있습니다.

어떻게 아느냐구요.. 이후 throttle을 올리면 모터가 돌아갑니다. (아.. 프로펠러 무조건 빼고 테스트 해야합니다 ^^)

![image](/assets/images/posts/6066688/c0028014_582b111f4184a.jpg)

자. Arming을 끄는건 아래와 같이 반대 방향으로 하면 됩니다.

![image](/assets/images/posts/6066688/c0028014_582b115f23d53.jpg)

자.. CC3D.. FLY SKY FS-T6 setting 참 쉽죠 ^^;

그럼 즐거운 비행 되세요~~