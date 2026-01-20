# Tomato Segmentation PoC

토마토 숙도(완숙/반숙/미숙) 판별과 질병 영역 탐지를 Segmentation 기반으로 수행하고, Streamlit 대시보드에서 오버레이 형태로 시각화하는 PoC입니다.

## 개요

- 숙도(열매)와 질병(잎/열매 병반) Segmentation 모델을 각각 독립 학습합니다.
- 추론 시 두 모델 결과를 하나의 이미지로 합쳐 직관적으로 확인합니다.
- 결과는 웹 대시보드에서 시각화합니다.

## 데이터셋

- Laboro Tomato (COCO instance segmentation): https://www.kaggle.com/datasets/nexuswho/laboro-tomato
  - 원본 6클래스(크기 + 숙도)유지
- PlantSeg Tomato subset (LabelMe polygons): https://zenodo.org/records/13762907
  - 다작물 질병 데이터셋에서 토마토만 선별한 서브셋입니다.

## 모델

- YOLOv8-seg: 베이스라인 Segmentation 모델.
- YOLOv11-seg: 소형/복잡 객체에 유리한 개선형 모델.
- SegNeXt: 병반 경계가 불규칙한 영역에 강한 Transformer 기반 모델.

## 파이프라인

1. 이미지 해상도 정규화.
2. PlantSeg 토마토 서브셋을 COCO 포맷으로 변환.
3. 숙도 모델과 질병 모델을 독립적으로 학습.
4. 질병 학습 시 클래스 불균형 대응(가중치/오버샘플링/Focal loss).
5. 단일 이미지에 두 모델을 추론하고 결과를 오버레이.
6. Streamlit 대시보드에서 시각화.

## 레포 구조

```
.
+-- notebook/
|   +-- Laboro_yolov8m.ipynb
|   +-- plantseg_v11.ipynb
|   +-- Tomato_Final.ipynb
+-- result/
|   +-- IMG_0986_analyzed.png
|   +-- IMG_0987_analyzed.png
|   +-- IMG_0988_analyzed.png
|   +-- tomato_early_blight_1_analyzed.png
|   +-- jpg/
|       +-- (original JPG outputs)
+-- LICENSE
+-- README.md
```

## 노트북

- `notebook/Laboro_yolov8m.ipynb`: Laboro 숙도 Segmentation 학습(YOLOv8m).
- `notebook/plantseg_v11.ipynb`: PlantSeg 토마토 질병 Segmentation 학습(YOLOv11-seg).
- `notebook/Tomato_Final.ipynb`: 숙도+질병 통합 추론 및 오버레이 시각화("Tomato Doctor").

## 결과

- PNG 오버레이 결과는 `result/`에 저장됩니다.
- 원본 JPG 출력은 `result/jpg/`에 보관됩니다.
