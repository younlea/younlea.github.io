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

// ================= [CONFIGURATION AREA] =================
const int SERVO_COUNT = 20;       // Total number of servos
const float US_PER_DEGREE = 10.3; // Pulse width change per 1 degree

// [SAFETY GUARD] Physical PWM limits
const int MIN_SAFE_PWM = 600;     
const int MAX_SAFE_PWM = 2400;

Servo myServos[SERVO_COUNT];

// Digital Pins for Servos (32 to 51)
// Internal Index 0 -> Pin 32 (User Motor 1)
// Internal Index 19 -> Pin 51 (User Motor 20)
const int servoPins[SERVO_COUNT] = {
  32, 33, 34, 35, 36, 37, 38, 39, 40, 41,
  42, 43, 44, 45, 46, 47, 48, 49, 50, 51
};

// [IMPORTANT] Calibration: PWM value at '0 degree' (Updated by User)
// Index 0 = Motor 1, Index 1 = Motor 2 ... Index 19 = Motor 20
int zeroOffsets[SERVO_COUNT] = {
  1700, 1000, 1600, 2400, 1000, // Motor 1 ~ 5
  1600, 1990, 2140, 900,  1800, // Motor 6 ~ 10
  1900, 2000, 1800, 1000, 2150, // Motor 11 ~ 15
  2150, 960,  1000, 1800, 2120  // Motor 16 ~ 20
};

void setup() {
  Serial.begin(9600);
  Serial.setTimeout(10);

  for (int i = 0; i < SERVO_COUNT; i++) {
    myServos[i].attach(servoPins[i]);
    
    // Move all motors to their calibrated 0 degree position
    setServoAngle(i, 0);
  }

  Serial.println("=== Robot Hand Controller Ready ===");
  Serial.println("Command Example: M1,90E (Motor 1 to 90 deg)");
  Serial.println("Range: Motor 1 ~ 20");
}

void loop() {
  if (Serial.available() > 0) {
    char c = Serial.read();

    if (c == 'M') {
      // 1. Parse User Motor ID (1 ~ 20)
      int inputId = Serial.parseInt(); 
      
      // 2. Parse Angle
      int angle = Serial.parseInt();

      // 3. Check End Packet
      char endChar = Serial.read(); 
      
      if (endChar == 'E') {
        // Convert User ID (1~20) to Array Index (0~19)
        int motorIndex = inputId - 1; 
        
        executeCommand(motorIndex, angle);
      } else {
        Serial.println("Error: Invalid Packet (Missing 'E')");
      }
    }
  }
}

// Execute command
void executeCommand(int index, int angle) {
  // Validate Array Index (0 ~ 19)
  if (index < 0 || index >= SERVO_COUNT) {
    Serial.print("Error: Invalid Motor ID. Use 1 to ");
    Serial.println(SERVO_COUNT);
    return;
  }

  int appliedPwm = setServoAngle(index, angle);

  // Feedback (Display as Motor 1 ~ 20)
  Serial.print("OK: Motor ");
  Serial.print(index + 1); 
  Serial.print(" -> ");
  Serial.print(angle);
  Serial.print(" deg (PWM:");
  Serial.print(appliedPwm);
  Serial.println(")");
}

// Core Logic (Uses Internal Index 0~19)
int setServoAngle(int index, int angle) {
  int centerPwm = zeroOffsets[index];
  
  // Calculate target PWM
  int pwmDelta = (int)(angle * US_PER_DEGREE);
  int targetPwm = centerPwm + pwmDelta;

  // Safety Guard: Constrain PWM to prevent damage
  // Note: Some motors (like Motor 4 with 2400) are already at the upper limit.
  // Positive angles for Motor 4 will be capped at 2400.
  int safePwm = constrain(targetPwm, MIN_SAFE_PWM, MAX_SAFE_PWM);

  myServos[index].writeMicroseconds(safePwm);
  
  return safePwm;
}


```

```c++
#include <Servo.h>

// ================= [설정 영역] =================
const int SERVO_COUNT = 20;       
const float US_PER_DEGREE = 10.3; // 1도당 펄스 변화량

// [안전 범위] 500 ~ 2500 (모터 4번 등 2400us 모터를 위해 필수)
const int MIN_SAFE_PWM = 500;     
const int MAX_SAFE_PWM = 2500;    

Servo myServos[SERVO_COUNT];

// 핀 번호 (32 ~ 51)
const int servoPins[SERVO_COUNT] = {
  32, 33, 34, 35, 36, 37, 38, 39, 40, 41,
  42, 43, 44, 45, 46, 47, 48, 49, 50, 51
};

// 0도(손을 편 상태)일 때의 PWM 값 (캘리브레이션 데이터)
int zeroOffsets[SERVO_COUNT] = {
  1700, 1000, 1600, 2400, 1000, 
  1600, 1990, 2140, 900,  1800, 
  1900, 2000, 1800, 1000, 2150, 
  2150, 960,  1000, 1800, 2120  
};

void setup() {
  Serial.begin(9600);
  
  // [핵심 수정] 초기화 루프
  for (int i = 0; i < SERVO_COUNT; i++) {
    
    // 1. attach 하기 "전"에 초기값(0점)을 먼저 입력합니다.
    // 이렇게 하면 전원이 들어올 때 1500(중간)을 거치지 않고 바로 제자리를 잡습니다.
    myServos[i].writeMicroseconds(zeroOffsets[i]);
    
    // 2. 그 다음 핀을 연결합니다. (범위는 500~2500으로 확장)
    // 이제 연결되는 순간 바로 위에서 설정한 zeroOffsets 값으로 힘을 줍니다.
    myServos[i].attach(servoPins[i], 500, 2500);
  }

  Serial.println("=== Robot Hand Ready (No Initial Jerk) ===");
}

void loop() {
  if (Serial.available() > 0) {
    char c = Serial.read(); 
    if (c == 'M') {
      int inputId = Serial.parseInt(); 
      int angle = Serial.parseInt(); // 음수(-90)도 그대로 받음
      char endChar = Serial.read(); 
      
      if (endChar == 'E') {
        executeCommand(inputId - 1, angle);
      }
    }
  }
}

void executeCommand(int index, int angle) {
  if (index < 0 || index >= SERVO_COUNT) return;
  
  int appliedPwm = setServoAngle(index, angle);
  
  Serial.print("M"); Serial.print(index + 1); 
  Serial.print(" Angle:"); Serial.print(angle);
  Serial.print(" -> PWM:"); Serial.println(appliedPwm);
}

int setServoAngle(int index, int angle) {
  int centerPwm = zeroOffsets[index];
  
  // [방향 배열 제거] 
  // 입력된 angle의 부호(+/-)에 따라 그대로 더하거나 뺍니다.
  // 예: 2400인 모터에 -90도가 들어오면 2400 + (-927) = 1473 (정상)
  int pwmDelta = (int)(angle * US_PER_DEGREE);
  int targetPwm = centerPwm + pwmDelta;

  // 안전 범위 적용 (500 ~ 2500)
  int safePwm = constrain(targetPwm, MIN_SAFE_PWM, MAX_SAFE_PWM);

  myServos[index].writeMicroseconds(safePwm);
  
  return safePwm;
}
```
