---
title: "[YOLO 활용] COCO 데이터셋에 없는 새로운 객체 추가 학습 가이드"
excerpt_separator: "<!--more-->"
categories:
  - AI
tags:
  - write here

toc : true
toc_sticky : true
---

# deepseek answer
```markdown

# YOLO 모델에 새로운 객체 추가 학습하기

YOLO(You Only Look Once)는 실시간 객체 감지 모델로, COCO 데이터셋을 기반으로 80개의 클래스를 기본적으로 인식합니다.  
하지만 **사용자 정의 객체를 추가 학습**해야 하는 경우가 많습니다.  
본 가이드에서는 YOLOv5/v8을 기준으로 COCO에 없는 객체를 학습시키는 방법을 단계별로 설명합니다.

---

## 1. 데이터셋 준비

### 1.1 이미지 수집 및 라벨링
- **이미지 요구사항**  
  - 최소 200~500장 이상 (다양한 각도/조명/배경 권장)
  - 해상도: 640x640 이상 권장
- **라벨링 도구**  
  ```markdown
  - [LabelImg](https://github.com/heartexlabs/labelImg) : 오프라인용
  - [CVAT](https://cvat.org/) : 웹 기반
  - [Roboflow](https://roboflow.com/) : 클라우드 통합 솔루션
  ```
- **YOLO 포맷 저장**  
  ```txt
  # 라벨 파일 예시 (your_object.txt)
  0 0.483 0.512 0.125 0.356  # [class_id x_center y_center width height]
  ```

### 1.2 데이터셋 구조
```bash
custom_data/
├── images/
│   ├── train/ # 학습 이미지 (80%)
│   └── val/   # 검증 이미지 (20%)
└── labels/
    ├── train/ 
    └── val/
```

### 1.3 YAML 설정 파일 생성
`custom_data.yaml` 작성:
```yaml
path: ./custom_data
train: images/train
val: images/val
names: 
  0: your_custom_object  # 클래스 이름
  1: another_object      # 추가 클래스 시 확장
```

---

## 2. 모델 학습 설정

### 2.1 사전 학습 모델 선택
```python
# YOLOv5 예시
model = torch.hub.load('ultralytics/yolov5', 'yolov5s', pretrained=True)
```

### 2.2 클래스 수 조정
- 모델 구성 파일(`models/yolov5s.yaml`) 수정:  
  ```yaml
  nc: 81  # COCO 80개 + 새 클래스 1개
  ```

### 2.3 학습 명령어
**YOLOv5**:
```bash
python train.py \
--img 640 \
--batch 16 \
--epochs 100 \
--data custom_data.yaml \
--cfg models/yolov5s.yaml \
--weights yolov5s.pt \
--name custom_train
```

**YOLOv8**:
```bash
yolo detect train \
data=custom_data.yaml \
model=yolov8n.pt \
epochs=100 \
imgsz=640 \
project=custom_train
```

---

## 3. 학습 최적화 팁

### 3.1 데이터 증강(Augmentation)
```yaml
# YOLOv5 기본 증강 (자동 적용)
augment: True  # Mosaic, Flip, Rotate 등
```

### 3.2 하이퍼파라미터 튜닝
```markdown
- 학습률: `--lr 0.01` → `0.001`로 점진적 조정
- Optimizer: SGD(default) → Adam 시도
- Early Stopping: `--patience 10` (과적합 방지)
```

### 3.3 GPU 활용
```bash
--device 0       # GPU 1개 사용
--device 0,1     # 멀티 GPU
```

---

## 4. 학습 결과 확인

### 4.1 성능 지표
- **mAP(mean Average Precision)**: `val.py` 실행 후 확인
- 학습 진행 상황: `tensorboard --logdir runs/train`

### 4.2 추론 테스트
```python
from ultralytics import YOLO

model = YOLO('runs/train/custom_train/weights/best.pt')
results = model.predict('test.jpg', conf=0.5)
results.show()
```

---

## 5. 문제 해결 가이드

### 5.1 일반적 이슈
| 증상 | 해결책 |
|-------|--------|
| **NaN Loss** | 학습률 감소(`--lr 0.001`), 배치 크기 축소 |
| **저사양 GPU 메모리 부족** | `--img 416`, `--batch 8`로 축소 |
| **새 객체 검출 실패** | 데이터 품질 검토, 에폭 증가 |

### 5.2 참고 자료
- [YOLOv5 공식 문서](https://github.com/ultralytics/yolov5)
- [YOLOv8 GitHub](https://github.com/ultralytics/ultralytics)

---

## 마치며
YOLO의 Transfer Learning 기능을 활용하면 **소량의 데이터로도 높은 정확도**의 커스텀 객체 감지기를 구축할 수 있습니다.  
실제 프로젝트 적용 시 **데이터 품질 관리**와 **하이퍼파라미터 실험**이 가장 중요한 요소입니다.  

궁금한 점이 있다면 댓글로 문의해주세요! ✨
```

이 Markdown 파일은 GitHub Pages, Hugo, Jekyll 등 대부분의 정적 사이트 생성기와 호환됩니다.  
필요에 따라 이미지나 추가 섹션을 삽입하여 보완할 수 있습니다.
```
# gpt4o 답변

# YOLO 추가 학습을 위한 데이터셋 준비 및 학습 가이드

YOLO 모델을 사용하여 새로운 객체를 탐지하려면, 먼저 적절한 데이터셋을 준비하고, 이를 기반으로 모델을 학습시켜야 합니다. 이 글에서는 데이터셋 준비부터 학습 및 결과 확인까지의 전체 과정을 단계별로 설명합니다.

---

## **1. 데이터셋 준비**

### **1-1. 이미지 수집**
먼저 학습하려는 객체가 포함된 이미지를 수집해야 합니다. 다음을 고려하세요:
- 다양한 배경, 조명 조건, 각도에서 이미지를 확보합니다.
- 가능한 한 다양한 환경에서 객체를 포함한 이미지를 많이 확보할수록 성능이 좋아집니다.

### **1-2. 이미지 라벨링**
수집한 이미지에 대해 객체의 위치를 지정하고 라벨링을 합니다. YOLO 모델은 특정 형식의 라벨 데이터를 요구하므로, 다음 도구를 사용해 라벨링 작업을 진행합니다:
- **LabelImg**: 가장 널리 사용되는 라벨링 도구.
- **Roboflow Annotate**: 웹 기반 라벨링 도구.

#### YOLO 라벨 형식
YOLO의 라벨 형식은 다음과 같습니다:
```
<class_id> <x_center> <y_center> <width> <height>
```
- `class_id`: 클래스 ID (0부터 시작).
- `x_center`, `y_center`: 경계 상자의 중심 좌표(이미지 너비와 높이를 기준으로 0~1 사이 값).
- `width`, `height`: 경계 상자의 너비와 높이(이미지 너비와 높이를 기준으로 0~1 사이 값).

### **1-3. 데이터셋 폴더 구조**
라벨링이 완료되면 데이터를 아래와 같은 구조로 정리합니다:
```
dataset/
├── images/
│   ├── train/  # 학습용 이미지
│   ├── val/    # 검증용 이미지
├── labels/
│   ├── train/  # 학습용 라벨
│   ├── val/    # 검증용 라벨
```

### **1-4. 데이터셋 설정 파일 작성**
YOLO 모델은 데이터셋 정보를 담고 있는 `.yaml` 파일이 필요합니다. 예시는 다음과 같습니다:

```yaml
train: dataset/images/train  # 학습 이미지 경로
val: dataset/images/val      # 검증 이미지 경로

nc: 1                        # 클래스 수
names: ['custom_object']     # 클래스 이름
```

---

## **2. 모델 학습**

YOLO 모델은 사전 학습된 가중치를 활용하여 추가 학습(Fine-tuning)을 진행합니다. 여기에서는 YOLOv8을 기준으로 설명합니다.

### **2-1. 환경 설정**
먼저 YOLOv8 라이브러리를 설치합니다:
```bash
pip install ultralytics
```

### **2-2. 학습 실행**
CLI 명령어 또는 Python 코드를 사용하여 모델을 학습시킬 수 있습니다.

#### CLI 명령어로 학습 실행:
```bash
yolo task=detect mode=train model=yolov8n.pt data=dataset/data.yaml epochs=50 imgsz=640 batch=16 name=custom_model
```
- `model`: 사전 학습된 YOLO 가중치 파일(e.g., `yolov8n.pt`).
- `data`: 데이터셋 설정 파일 경로.
- `epochs`: 학습 반복 횟수.
- `imgsz`: 입력 이미지 크기.
- `batch`: 배치 크기.

#### Python 코드로 학습 실행:
```python
from ultralytics import YOLO

# 모델 로드 및 학습 시작
model = YOLO('yolov8n.pt')  # 사전 학습된 가중치 로드
model.train(data='dataset/data.yaml', epochs=50, imgsz=640, batch=16)
```

---

## **3. 결과 확인**

### **3-1. 학습 결과**
학습이 완료되면 결과는 기본적으로 다음 경로에 저장됩니다:
```
runs/detect/train/
├── weights/
│   ├── best.pt  # 가장 성능이 좋은 모델 가중치 파일
│   ├── last.pt  # 마지막 에포크의 모델 가중치 파일
├── results.png  # 학습 과정 시각화 그래프 (Loss, mAP 등)
```

### **3-2. 테스트 실행**
학습된 모델(`best.pt`)을 사용하여 새로운 이미지나 비디오에서 객체를 탐지할 수 있습니다.

#### CLI 명령어로 테스트 실행:
```bash
yolo task=detect mode=predict model=runs/detect/train/weights/best.pt source=path/to/test/images
```

#### Python 코드로 테스트 실행:
```python
results = model.predict(source='path/to/test/images', save=True)
```
탐지 결과는 지정된 경로에 저장되며, 탐지된 객체가 경계 상자로 표시됩니다.

---

## **4. 추가 팁**

1. **데이터 증강(Augmentation)**: 다양한 상황에 대한 일반화 성능을 높이기 위해 데이터 증강 기법(회전, 크롭, 밝기 조정 등)을 활용하세요.
2. **하이퍼파라미터 튜닝**: 학습률, 배치 크기 등 하이퍼파라미터를 조정하여 성능을 최적화하세요.
3. **검증(mAP 확인)**: 검증 데이터에 대한 mAP(mean Average Precision)를 확인하여 모델 성능을 평가하세요.

---

## **5. 결론**

위 과정을 통해 YOLO 모델을 새로운 객체에 대해 추가 학습시킬 수 있습니다. 데이터 준비부터 결과 확인까지의 전체 흐름은 아래와 같습니다:

1. 이미지 수집 및 라벨링 → 데이터셋 구성.
2. YOLO 환경 설정 및 모델 학습.
3. 테스트를 통해 결과 확인.

이 과정을 반복적으로 수행하며 데이터를 보완하면 더 높은 정확도를 얻을 수 있습니다!

출처

