---
title: "meta sam - m1 test 하기 "
excerpt_separator: "<!--more-->"
categories:
  - AI
tags:
  - write here

toc : true
toc_sticky : true
---

# meta sam


# 환경 설정
1. miniconda로 환경 설정
```
conda create -n sam python=3.10
conda activate sam
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu
pip install numpy matplotlib Pillow
git clone https://github.com/facebookresearch/segment-anything-2.git
cd segment-anything-2
pip install -e .
pip install opencv-python

```

checkpoint 받기 
```
ViT-H (vit_h): 가장 크고 성능이 높은 모델.
wget -P checkpoints https://dl.fbaipublicfiles.com/segment_anything/sam_vit_h_4b8939.pth

ViT-L (vit_l): 중간 크기와 성능.
wget -P checkpoints https://dl.fbaipublicfiles.com/segment_anything/sam_vit_l_0b3195.pth

ViT-B (vit_b): 가장 작은 모델로, 빠르고 효율적.
wget -P checkpoints https://dl.fbaipublicfiles.com/segment_anything/sam_vit_b_01ec64.pth

```
segment-anything package 설치 
```
pip install git+https://github.com/facebookresearch/segment-anything.git
```

# sample code on macbook air m1
## 기본 모듈 오픈후 동작 코드
```python
import cv2
import numpy as np
from segment_anything import sam_model_registry, SamPredictor

# 모델 체크포인트 경로 및 설정
checkpoint_path = "segment-anything-2/checkpoints/sam_vit_b_01ec64.pth"  # ViT-B 모델 체크포인트 경로
model_type = "vit_b"  # 모델 타입 (ViT-B)

# SAM 모델 로드
print("Loading SAM model...")
sam_model = sam_model_registry[model_type](checkpoint=checkpoint_path)
predictor = SamPredictor(sam_model)

# 웹캠 열기
cap = cv2.VideoCapture(0)
if not cap.isOpened():
    print("웹캠을 열 수 없습니다. 다른 프로그램에서 사용 중인지 확인하세요.")
    exit()

print("웹캠이 열렸습니다. 'q'를 눌러 종료하세요.")

while True:
    ret, frame = cap.read()
    
    # 프레임 읽기 실패 시 경고 출력 후 계속 시도
    if not ret:
        print("프레임을 읽을 수 없습니다. 다시 시도합니다...")
        continue

    # OpenCV 이미지를 RGB로 변환 (SAM은 RGB 이미지를 사용)
    image_rgb = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)

    # 이미지 입력 설정
    try:
        predictor.set_image(image_rgb)

        # 임의의 포인트와 박스를 사용해 세그먼트 예측 (데모용)
        input_point = np.array([[frame.shape[1] // 2, frame.shape[0] // 2]])  # 중앙점
        input_label = np.array([1])  # 포인트 레이블 (1: foreground)
        masks, _, _ = predictor.predict(point_coords=input_point, point_labels=input_label, box=None)
    except Exception as e:
        print(f"세그먼트 예측 중 오류 발생: {e}")
        continue

    # 마스크 시각화
    if masks is not None and len(masks) > 0:
        mask = masks[0]  # 첫 번째 마스크만 사용 (다중 객체 세그먼트 가능)
        mask_image = (mask * 255).astype(np.uint8)  # 마스크를 0~255로 변환
        mask_colored = cv2.applyColorMap(mask_image, cv2.COLORMAP_JET)  # 컬러 맵 적용
        overlay = cv2.addWeighted(frame, 0.7, mask_colored, 0.3, 0)  # 원본 위에 마스크 오버레이
        cv2.imshow("Segment Anything Model - Mask Overlay", overlay)
    else:
        cv2.imshow("Segment Anything Model", frame)

    # 'q' 키를 누르면 종료
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

# 자원 해제
cap.release()
cv2.destroyAllWindows()

```

# 중간에 있는 물체를 segment하는 예제 코드 
아래 폴더 구조로 두개 모델 파일을 받아야 함.    
```
ai_streaming_test/
├── sam_sample_code.py
├── models/
│   ├── MobileNetSSD_deploy.prototxt
│   ├── MobileNetSSD_deploy.caffemodel
```

중간에 있는 물체를 기준으로 segment하는 예제 코드    
```pyrhon
import cv2
import numpy as np
from segment_anything import sam_model_registry, SamPredictor

# 모델 체크포인트 경로 및 설정
checkpoint_path = "segment-anything-2/checkpoints/sam_vit_b_01ec64.pth"  # ViT-B 모델 체크포인트 경로
model_type = "vit_b"  # 모델 타입 (ViT-B)

# SAM 모델 로드
print("Loading SAM model...")
sam_model = sam_model_registry[model_type](checkpoint=checkpoint_path)
predictor = SamPredictor(sam_model)

# MobileNet-SSD 모델 파일 경로 설정
prototxt_path = "models/MobileNetSSD_deploy.prototxt"
caffemodel_path = "models/MobileNetSSD_deploy.caffemodel"

# MobileNet-SSD 네트워크 로드
print("Loading MobileNet-SSD model...")
net = cv2.dnn.readNetFromCaffe(prototxt_path, caffemodel_path)

# 웹캠 열기
cap = cv2.VideoCapture(0)
if not cap.isOpened():
    print("웹캠을 열 수 없습니다. 다른 프로그램에서 사용 중인지 확인하세요.")
    exit()

print("웹캠이 열렸습니다. 'q'를 눌러 종료하세요.")

while True:
    ret, frame = cap.read()
    
    if not ret:
        print("프레임을 읽을 수 없습니다. 다시 시도합니다...")
        continue

    # OpenCV 이미지를 RGB로 변환 (SAM은 RGB 이미지를 사용)
    image_rgb = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)

    # 중앙점 계산
    center_x, center_y = frame.shape[1] // 2, frame.shape[0] // 2

    # SAM 세그멘테이션 수행
    try:
        predictor.set_image(image_rgb)
        input_point = np.array([[center_x, center_y]])  # 중앙점
        input_label = np.array([1])  # 포인트 레이블 (1: foreground)
        masks, _, _ = predictor.predict(point_coords=input_point, point_labels=input_label, box=None)
    except Exception as e:
        print(f"세그먼트 예측 중 오류 발생: {e}")
        continue

    if masks is not None and len(masks) > 0:
        mask = masks[0]  # 첫 번째 마스크만 사용 (다중 객체 세그먼트 가능)
        mask_image = (mask * 255).astype(np.uint8)  # 마스크를 0~255로 변환

        # 마스크 영역 추출
        segmented_object = cv2.bitwise_and(frame, frame, mask=mask_image)

        # YOLO 또는 MobileNet SSD를 사용한 객체 탐지 수행
        blob = cv2.dnn.blobFromImage(segmented_object, scalefactor=0.007843, size=(300, 300), mean=(127.5, 127.5, 127.5))
        net.setInput(blob)
        detections = net.forward()

        object_name = None

        for i in range(detections.shape[2]):
            confidence = detections[0, 0, i, 2]
            if confidence > 0.5:  # 신뢰도가 높은 경우만 처리
                class_id = int(detections[0, 0, i, 1])
                object_name = coco_labels[class_id] if class_id < len(coco_labels) else 'Unknown'
                break

        # 마스크 시각화 및 객체 이름 표시
        mask_colored = cv2.applyColorMap(mask_image, cv2.COLORMAP_JET)  # 컬러 맵 적용
        overlay = cv2.addWeighted(frame, 0.7, mask_colored, 0.3, 0)  # 원본 위에 마스크 오버레이

        if object_name:
            cv2.putText(overlay, object_name, (center_x - 50, center_y - 50), cv2.FONT_HERSHEY_SIMPLEX,
                        fontScale=1.0, color=(255, 255, 255), thickness=2)

        cv2.imshow("Segment Anything Model - Mask Overlay with Object Name", overlay)
    else:
        cv2.imshow("Segment Anything Model - No Segmentation Found!", frame)

    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()

```
