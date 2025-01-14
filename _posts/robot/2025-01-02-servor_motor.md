---
title: "다이나믹셀 AX-12A 셋팅하기"
excerpt_separator: "<!--more-->"
categories:
  - robot
tags:
  - robot

toc : true
toc_sticky : true
---

다이나믹셀 AX-12A를 초기 설정하고 통신을 시작하려면 아래 단계를 따라 진행하면 됩니다. 이 과정은 모터 ID 설정, 각도 조절, 현재 각도 확인 등의 기본적인 작업을 포함하며, 4족 보행 로봇 제작에 필요한 기초 작업입니다.

---

## **AX-12A 초기 설정 방법**

### **1. 기본 준비물**
- **AX-12A 다이나믹셀 모터**
- 제어기 (예: OpenCM9.04, Arduino 등)
- TTL 통신용 USB2Dynamixel 또는 Dynamixel Shield
- 전원 공급 장치 (권장 전압: 11.1V)
- PC와 연결할 USB 케이블
- 로보티즈에서 제공하는 소프트웨어 (예: Dynamixel Wizard 2.0)

---

### **2. 초기 설정**
#### **(1) ID 설정**
모든 AX-12A는 출하시 기본 ID가 1로 설정되어 있습니다. 여러 모터를 사용할 경우 각 모터의 ID를 고유하게 변경해야 합니다.
1. **Dynamixel Wizard 2.0** 프로그램 설치 및 실행. [Dynamixel wizard 2.0](https://emanual.robotis.com/docs/en/software/dynamixel/dynamixel_wizard2/)   
2. AX-12A를 제어기에 연결 후 PC와 연결. [USB2Dynamicxel](https://emanual.robotis.com/docs/en/parts/interface/usb2dynamixel/)    
3. 프로그램에서 "Scan" 기능을 사용해 연결된 모터를 검색.
4. 검색된 모터의 ID를 선택하고 원하는 ID로 변경.
   - ID 범위: 0~253 (254는 Broadcast ID로 사용됨).
   - 변경 후 저장.

#### **(2) 통신 속도(Baud Rate) 설정**
기본 통신 속도는 1Mbps로 설정되어 있습니다. 필요에 따라 Dynamixel Wizard에서 속도를 조정할 수 있습니다.
- 지원 속도 범위: 7,843bps ~ 1Mbps.

---

### **3. 각도 조절 및 현재 각도 확인**
#### **(1) 각도 조절**
AX-12A는 관절 모드(0~300°)와 바퀴 모드(무한 회전)를 지원합니다.
1. 관절 모드에서 목표 각도(Goal Position)를 설정합니다.
   - 목표 각도의 범위: 0~1023 (0°~300°에 대응).
   - 예제 코드:
     ```cpp
     dxl.setGoalPosition(ID, 목표값);
     ```
2. 바퀴 모드로 전환하려면 EEPROM의 "CW Angle Limit"과 "CCW Angle Limit" 값을 0으로 설정합니다.

#### **(2) 현재 각도 확인**
모터의 현재 위치(Present Position)를 읽어 원하는 동작을 확인합니다.
- 예제 코드:
  ```cpp
  int current_position = dxl.getPresentPosition(ID);
  Serial.print("현재 위치: ");
  Serial.println(current_position);
  ```

---

### **4. 토크(Torque) 제어**
초기 설정 중에는 토크를 비활성화해야 EEPROM 값을 안전하게 변경할 수 있습니다.
- 토크 비활성화:
  ```cpp
  dxl.torqueOff(ID);
  ```
- 토크 활성화:
  ```cpp
  dxl.torqueOn(ID);
  ```

---

## **참고 자료 및 유용한 링크**
### **관련 가이드**
1. [로보티즈 e-Manual](https://emanual.robotis.com/docs/kr/dxl/ax/ax-12a/)에서 AX-12A의 상세한 사용법과 프로토콜 설명을 확인할 수 있습니다[6].
2. 네이버 블로그의 [4족 보행 로봇 제작 가이드](https://blog.naver.com/PostView.nhn?blogId=thumbdown&logNo=220335782684)는 다이나믹셀 ID 변경 및 기본 설정 방법을 다룹니다[2].

### **유튜브 추천**
1. [Dynamixel AX-12A Servos for Beginners](https://www.youtube.com/watch?v=tkV9CPWjtgU): 초보자를 위한 AX-12A 사용법 강좌입니다[4].

---

이 단계를 완료하면 AX-12A의 기본 셋업이 끝나며, 이를 바탕으로 로봇의 동작을 프로그래밍하고 제어할 수 있습니다. 추가적으로 궁금한 점이 있다면 로보티즈 포럼이나 e-Manual을 참고하세요!

Citations:
[1] https://velog.io/@mincheol710313/ROS-2-Dynamixel-AX-12A-%EB%8F%99%EC%8B%9C-%EA%B5%AC%EB%8F%99
[2] https://blog.naver.com/PostView.nhn?blogId=thumbdown&logNo=220335782684
[3] https://eteo.tistory.com/169
[4] https://www.youtube.com/watch?v=tkV9CPWjtgU
[5] https://www.robotis.com/service/forum_view.php?bbs_no=2588750
[6] https://emanual.robotis.com/docs/kr/dxl/ax/ax-12a/
[7] https://dynamixel.com/service/forum_view.php?bbs_no=9761&page=649
[8] https://blog.naver.com/nanotoly/221910252560
[9] https://www.robotis.com/service/forum_view.php?bbs_no=662840&page=256

