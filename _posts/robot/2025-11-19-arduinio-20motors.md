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
const float US_PER_DEGREE = 10.3; 

// [안전 범위] 500 ~ 2500
const int MIN_SAFE_PWM = 500;     
const int MAX_SAFE_PWM = 2500;    

Servo myServos[SERVO_COUNT];

// 핀 번호: 아두이노에는 32번부터 51번까지 순서대로 연결됨
const int servoPins[SERVO_COUNT] = {
  32, 33, 34, 35, 36, 37, 38, 39, 40, 41,
  42, 43, 44, 45, 46, 47, 48, 49, 50, 51
};

// [재배열된 0점 데이터]
// 사용자가 기록한 순서(32,33, 42,43...)를 핀 번호 순서(32,33,34...)로 다시 정렬함
int zeroOffsets[SERVO_COUNT] = {
  // Pin 32~41 (앞쪽 10개)
  1700, // Pin 32 (기록순서 1번째 값)
  1000, // Pin 33 (기록순서 2번째 값)
  1000, // Pin 34 (기록순서 5번째 값)
  1600, // Pin 35 (기록순서 6번째 값)
  900,  // Pin 36 (기록순서 9번째 값)
  1800, // Pin 37 (기록순서 10번째 값)
  1800, // Pin 38 (기록순서 13번째 값)
  1000, // Pin 39 (기록순서 14번째 값)
  960,  // Pin 40 (기록순서 17번째 값)
  1000, // Pin 41 (기록순서 18번째 값)

  // Pin 42~51 (뒤쪽 10개)
  1600, // Pin 42 (기록순서 3번째 값)
  2400, // Pin 43 (기록순서 4번째 값)
  1990, // Pin 44 (기록순서 7번째 값 - 오타추정 부분)
  2140, // Pin 45 (기록순서 8번째 값)
  1900, // Pin 46 (기록순서 11번째 값)
  2000, // Pin 47 (기록순서 12번째 값)
  2150, // Pin 48 (기록순서 15번째 값)
  2150, // Pin 49 (기록순서 16번째 값)
  1800, // Pin 50 (기록순서 19번째 값)
  2120  // Pin 51 (기록순서 20번째 값)
};

void setup() {
  Serial.begin(9600);
  
  // 초기화 루프
  for (int i = 0; i < SERVO_COUNT; i++) {
    
    // 1. [튀는 현상 방지] 연결 전에 초기값 먼저 입력
    myServos[i].writeMicroseconds(zeroOffsets[i]);
    
    // 2. 그 다음 연결 (범위 500~2500 확장)
    myServos[i].attach(servoPins[i], 500, 2500);
  }

  Serial.println("=== Robot Hand Controller Re-Mapped ===");
  Serial.println("Pin Order Fixed: 32...51 Linear");
}

void loop() {
  if (Serial.available() > 0) {
    char c = Serial.read(); 
    if (c == 'M') {
      int inputId = Serial.parseInt(); 
      int angle = Serial.parseInt(); // -90 ~ 180
      char endChar = Serial.read(); 
      
      if (endChar == 'E') {
        // 사용자는 M1~M20으로 명령 -> 코드 내부는 0~19 인덱스 사용
        executeCommand(inputId - 1, angle);
      }
    }
  }
}

void executeCommand(int index, int angle) {
  if (index < 0 || index >= SERVO_COUNT) return;
  
  int appliedPwm = setServoAngle(index, angle);
  
  // 확인용 출력
  Serial.print("Pin "); Serial.print(servoPins[index]); // 실제 핀번호 출력
  Serial.print(" (M"); Serial.print(index + 1); 
  Serial.print(") Angle:"); Serial.print(angle);
  Serial.print(" -> PWM:"); Serial.println(appliedPwm);
}

int setServoAngle(int index, int angle) {
  int centerPwm = zeroOffsets[index];
  
  // 입력된 각도만큼 더하거나 뺌 (음수 각도 지원)
  int pwmDelta = (int)(angle * US_PER_DEGREE);
  int targetPwm = centerPwm + pwmDelta;

  // 안전 범위 (500 ~ 2500)
  int safePwm = constrain(targetPwm, MIN_SAFE_PWM, MAX_SAFE_PWM);

  myServos[index].writeMicroseconds(safePwm);
  
  return safePwm;
}
```
