---
title: "Multiwii - source code 연동"
excerpt_separator: "<!--more-->"
date: 2015-02-10 00:39:36 +0900
categories:
  - develop_env
tags:
  - develop_env
  - embedded
  - 임베디드

toc : true
toc_sticky : true
---

multi wii source code를 아래에서 받고...

<https://code.google.com/p/multiwii/>

arduino code를 다운로드를 위해서 arduino 에서 로드해서 빌드하면 에러 난다..

ㅋ...

일단 config.h를 수정해야지만 빌드가 되도록 되어있고.. 자신에게 맞는걸 수정해야 한다.

(인터보드 까페에서 초기 셋팅하는 부분을 확인하고 나에게 맞춰서 수정을 하였다.)

config.h

비행체 선택하기

#define QUADX

I2C speed

#define I2C\_SPEED 400000L

센서 선택

#define MPU6050       //combo + ACC

센서 방향

/\* enforce your individual sensor orientation - even overrides board specific defaults \*/

#define FORCE\_ACC\_ORIENTATION(X, Y, Z)  {imu.accADC[ROLL]  =  Y; imu.accADC[PITCH]  = -X; imu.accADC[YAW]  = Z;}

#define FORCE\_GYRO\_ORIENTATION(X, Y, Z) {imu.gyroADC[ROLL] = X; imu.gyroADC[PITCH] = Y; imu.gyroADC[YAW] = -Z;}

MPU 6050 - LOW PASS FILTER

#define MPU6050\_LPF\_42HZ

PPM신호 종류 선택

#define SERIAL\_SUM\_PPM         ROLL,PITCH,THROTTLE,YAW,AUX1,AUX2,AUX3,AUX4,8,9,10,11 //For Robe/Hitec/Futaba

위 부분을 수정하면 일단 빌드되고 바이너리가 업로드 까지 된다..

여기서 두번째 이슈...

multiwii conf라고 PC에서 통신하여 calibration을 하는 프로그램이 있는데 실행이 안된다...

이게.. 해보니까... 내 system이 64bit라서 안되는거 였다.

32bit에서 하면 잘된다네.. ㅜㅜ 대충 multiwii wiki를 뒤져 보니 java 64 bit로 된 모듈에 문제가 있단다.

------------------------------------------------------------------------------------

그래서 가이드 하는게 아래 링크에 있는 chrome tool인데...

[https://chrome.google.com/webstore/deta ... figk?hl=en](https://chrome.google.com/webstore/detail/baseflight-configurator/mppkgnedeapfejgfimkdoninnofofigk?hl=en)

잘된다고 하는데 난 역시 연결 실패를 한다.. 하아..

어찌됐던 안되는거 하나 찾았으니 오늘도 한단계 업그레이드.. ^^;

내일은 기필코 뚫으리라..(문제는 집 PC가 64bit라... 훔... 리눅스 PC를 32bit로 설치해서 써보려고 해도.. . 자바랑 프로세싱 깔 생각을 하니.. 공부해야지..)

훔.. linux PC에서 실행해서 Mutiwii conf가 동작을 하는건 확인했는데.. 이것도 문제가 발생..

넷북이라 화면 해상도가 낮아서 다 안나온다 ㅜㅜ

일단 코드가 문제인듯 하니.. 코드 수정부터 해보자.. ^^;

------------------------------------------------------------------------------------

---->  위의 내용은 무시 하길.. ㅜㅜ

32bit 컴퓨터에서 연결하고 하니 잘 된다.. 가속도와 자이로 값 잘 나온다는... 저 chrome app은 다른데 쓰는건가? ㅡㅜ

![image](/assets/images/posts/5865733/c0028014_54f5d890461ae.jpg)

![image](/assets/images/posts/5865733/c0028014_54f5d8a438a76.jpg)

turnigy 연결하기....

PIN연결 부분을 못찾았었는데.. 코드 보니까.. Digital PIN2에 연결하란다.. ㅡ.ㅡ

![image](/assets/images/posts/5865733/c0028014_54f729a3d3dd4.png)

오.. start 하고 turnigy에서 신호 보내니까 아래 THROT, ROLL, PITCH, YAW 동작 한다...

![image](/assets/images/posts/5865733/c0028014_54f72d9f1e3ae.png)

이제 모터 연결을 해봐야 겠구나..

---> roll, pitch가 거꾸로 동작해서 아래와 같이 순서 바꿨다 ㅡ.ㅡ;

![image](/assets/images/posts/5865733/c0028014_55521efb43b89.png)

왼쪽 위아래 - pitch

좌우 - yaw

오른쪽 위아래 - throttle

좌우   - roll

\