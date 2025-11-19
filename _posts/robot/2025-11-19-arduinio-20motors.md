---
title: "meag2560으로 20개 서보모터제어하기(with pwm)"
excerpt_separator: "<!--more-->"
categories:
  - robot
tags:
  - robot

toc : true
toc_sticky : true
---


```c
#include <Servo.h>

// ================= [설정 구간] =================
const int SERVO_COUNT = 20;
const float US_PER_DEGREE = 10.3; // 1도당 PWM 변화량

// 안전 PWM 범위 (이 범위를 벗어나는 계산값은 강제로 이 값으로 고정됨)
const int MIN_SAFE_PWM = 600;
const int MAX_SAFE_PWM = 2400;

Servo myServos[SERVO_COUNT];

// 서보 핀 번호 (32 ~ 51)
const int servoPins[SERVO_COUNT] = {
  32, 33, 34, 35, 36, 37, 38, 39, 40, 41,
  42, 43, 44, 45, 46, 47, 48, 49, 50, 51
};

// [중요] 각 모터별 0도 기준 PWM 값 (캘리브레이션 값 입력 필수)
int zeroOffsets[SERVO_COUNT] = {
  1700, 1000, 1600, 2400, 1500, 1500, 1500, 1500, 1500, 1500,
  1500, 1500, 1500, 1500, 1500, 1500, 1500, 1500, 1500, 1500
};

void setup() {
  Serial.begin(9600);
  Serial.setTimeout(50); // 데이터 읽기 타임아웃 단축 (빠른 반응을 위해)

  // 서보 초기화
  for (int i = 0; i < SERVO_COUNT; i++) {
    myServos[i].attach(servoPins[i]);
    // 초기 위치: 0도로 설정
    setServoAngle(i, 0);
  }

  Serial.println("=== Servo Controller Ready ===");
  Serial.println("Command Format: M<ID>,<ANGLE>E");
  Serial.println("Example: M10,90E or M1,-10E");
}

void loop() {
  // 시리얼 버퍼에 데이터가 있는지 확인
  if (Serial.available() > 0) {
    char c = Serial.read();

    // 1. 시작 패킷 'M' 확인
    if (c == 'M') {
      // 2. 첫 번째 숫자 파싱 (모터 ID)
      int motorId = Serial.parseInt(); 
      
      // 3. 두 번째 숫자 파싱 (각도) - 중간의 쉼표나 공백은 parseInt가 알아서 무시함
      int angle = Serial.parseInt();

      // 4. 종료 패킷 'E' 확인 (데이터 무결성 검사)
      // parseInt 후 다음 문자를 읽어서 E인지 확인
      char endChar = Serial.read(); 
      
      if (endChar == 'E') {
        // 모든 형식이 맞으면 실행
        executeCommand(motorId, angle);
      } else {
        // E가 없으면 에러 처리 (혹은 줄바꿈 문자가 껴있을 경우를 대비해 유연하게 처리 가능)
        // 여기서는 엄격하게 E를 체크합니다.
        Serial.println("Error: Missing End Packet 'E'");
      }
    }
  }
}

// 명령 실행 및 결과 출력 함수
void executeCommand(int id, int angle) {
  // 모터 ID 유효성 검사
  if (id < 0 || id >= SERVO_COUNT) {
    Serial.print("Error: Invalid Motor ID ");
    Serial.println(id);
    return;
  }

  // 모터 제어 함수 호출
  int actualPwm = setServoAngle(id, angle);

  // 결과 시리얼 모니터로 피드백
  Serial.print("OK: Motor[");
  Serial.print(id);
  Serial.print("] -> ");
  Serial.print(angle);
  Serial.print(" deg (PWM: ");
  Serial.print(actualPwm);
  Serial.println(")");
}

// 각도를 PWM으로 변환하여 모터 제어 (이전 코드 활용)
int setServoAngle(int index, int angle) {
  int centerPwm = zeroOffsets[index];
  int pwmDelta = (int)(angle * US_PER_DEGREE);
  int targetPwm = centerPwm + pwmDelta;

  // 안전 범위 제한
  int safePwm = constrain(targetPwm, MIN_SAFE_PWM, MAX_SAFE_PWM);

  myServos[index].writeMicroseconds(safePwm);
  
  return safePwm; // 디버깅을 위해 실제 들어간 PWM 값 반환
}

```
