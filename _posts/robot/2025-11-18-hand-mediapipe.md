---
title: "Google Mediapipe model"
excerpt_separator: "<!--more-->"
categories:
  - robot
tags:
  - robot

toc : true
toc_sticky : true
---


# media pipe hand 사용 모션 캡쳐

```
conda create -n robot_env python=3.11
conda activate robot_env
pip install opencv-python mediapipe "numpy<2"

```
``` python
import cv2
import mediapipe as mp
import time

# 1. MediaPipe Hands 모델 설정
mp_hands = mp.solutions.hands
mp_drawing = mp.solutions.drawing_utils
mp_drawing_styles = mp.solutions.drawing_styles

# 로봇 제어용이므로 정확도를 위해 min_detection_confidence를 약간 높게 설정
hands = mp_hands.Hands(
    max_num_hands=1,            # 한 손만 제어한다면 1, 양손이면 2
    model_complexity=1,         # 0: 빠름, 1: 보통, 2: 정확함 (맥북에어는 1 추천)
    min_detection_confidence=0.7,
    min_tracking_confidence=0.7
)

# 2. 웹캠 캡처 초기화 (0번이 보통 기본 내장 카메라)
cap = cv2.VideoCapture(0)

# FPS 계산을 위한 변수
prev_time = 0

print("카메라를 시작합니다. 종료하려면 화면을 클릭하고 'q'를 누르세요.")

while cap.isOpened():
    success, image = cap.read()
    if not success:
        print("카메라 프레임을 찾을 수 없습니다.")
        continue

    # 3. 이미지 전처리
    # OpenCV는 BGR을 쓰지만, MediaPipe는 RGB를 원하므로 변환
    image = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)
    
    # 성능 향상을 위해 이미지를 '쓰기 불가'로 설정하고 처리
    image.flags.writeable = False
    results = hands.process(image)

    # 다시 BGR로 변환하여 화면에 출력 준비
    image.flags.writeable = True
    image = cv2.cvtColor(image, cv2.COLOR_RGB2BGR)

    # 4. 손 랜드마크 그리기 (관절 시각화)
    if results.multi_hand_landmarks:
        for hand_landmarks in results.multi_hand_landmarks:
            
            # 관절(Joint)과 뼈대(Connection) 스타일 지정
            # 관절: 초록색, 뼈대: 흰색
            mp_drawing.draw_landmarks(
                image,
                hand_landmarks,
                mp_hands.HAND_CONNECTIONS,
                mp_drawing.DrawingSpec(color=(0, 255, 0), thickness=4, circle_radius=4), # 관절(점)
                mp_drawing.DrawingSpec(color=(255, 255, 255), thickness=2, circle_radius=2) # 뼈대(선)
            )

            # (옵션) 각 손가락 끝(TIP)만 빨간색으로 강조하고 싶다면 아래 코드 활용 가능
            # h, w, c = image.shape
            # for id, lm in enumerate(hand_landmarks.landmark):
            #     # 4, 8, 12, 16, 20번이 손가락 끝
            #     if id in [4, 8, 12, 16, 20]:
            #         cx, cy = int(lm.x * w), int(lm.y * h)
            #         cv2.circle(image, (cx, cy), 10, (0, 0, 255), cv2.FILLED)

    # 5. 화면 출력 (거울 모드 적용 및 FPS 표시)
    image = cv2.flip(image, 1) # 거울처럼 좌우 반전
    
    curr_time = time.time()
    fps = 1 / (curr_time - prev_time)
    prev_time = curr_time
    
    cv2.putText(image, f'FPS: {int(fps)}', (10, 30), cv2.FONT_HERSHEY_SIMPLEX, 1, (0, 255, 0), 2)
    
    cv2.imshow('Hand Joint Visualizer', image)

    # 'q' 키를 누르면 종료
    if cv2.waitKey(5) & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()
```
