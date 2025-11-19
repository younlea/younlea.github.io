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
