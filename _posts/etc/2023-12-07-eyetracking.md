---
title: "eyetracking 장비 만들어 보기"
excerpt_separator: "<!--more-->"
categories:
  - etc
tags:
  - eyetracking

toc : true
toc_sticky : true
---

# 시중 제품
토비 아이트렉커 - 40만원대

내가 화면의 어디를 보고 있는지 알아내는 기술이 필요함. 

# 직접 만들기
노트북 카메라와 거리센서만 있으면 만들수 있지 않을까?

## eyetracking code
```python
import cv2
import numpy as np
import dlib #68개의 점으로 얼굴을 인식한다

cap = cv2.VideoCapture(0)
detector = dlib.get_frontal_face_detector()
predictor = dlib.shape_predictor("shape_predictor_68_face_landmarks.dat")

while True:
	_, frame = cap.read();
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
    faces = detector(gray)
    
    for face in faces:
    	x1, y1 = face.left(), face.top()
        x2, y2 = face.right(), face.bottom()
        
    cv2.rectangle(frame, (x1, y1), (x2, y2), (0, 255, 0), 2)
    cv2.imshow("Frame", frame)
    
    key = cv2.waitKey(1)
    
    if key == 27:
    	break
        
    cap.release()
    cv2.destroyAllWindows()
```
