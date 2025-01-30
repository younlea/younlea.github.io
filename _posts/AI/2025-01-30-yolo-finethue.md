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
