# 🍅 Tomato Segmentation PoC

토마토의 숙도(완숙/반숙/미숙)와 질병 병반을 Segmentation 모델로 정밀 탐지하고,  
분석 결과를 이미지 오버레이 및 직관적인 웹 UI로 제공하는 PoC 프로젝트입니다.

<table>
  <tr>
    <td align="center">
      <img src="result/Doctor_jpg/main.png" alt="웹 UI 화면" width="100%" />
      <br />
      <b>📸 Web UI Input</b>
    </td>
    <td align="center">
      <img src="result/Doctor_jpg/detection.png" alt="Segmentation 결과" width="100%" />
      <br />
      <b>🔍 Segmentation Result</b>
    </td>
  </tr>
</table>

## 🚀 주요 기능
- **다중 클래스 탐지**: 숙도 3단계 + 주요 질병 병반 동시 분석
- **실시간 오버레이**: 원본 영상 위에 마스크를 합성하여 시각화
- **Web Dashboard**: 사용자 친화적인 분석 결과 모니터링
## 🚀 개요

- Laboro Tomato COCO를 3-class 숙도 라벨로 정리해 YOLOv8m-seg 학습.
- PlantSeg Tomato LabelMe를 7-class 질병 라벨로 변환해 YOLOv11m-seg 학습.
- MMSegmentation(SegNeXt) 파이프라인으로 질병 Segmentation 대안 학습.
- YOLO + SAM2로 질병 경계 보정 및 통합 시각화.
- Streamlit 앱으로 진단 UI 제공(ngrok 임시 공개 링크).

## 📊 데이터셋

- Laboro Tomato (COCO instance segmentation): https://www.kaggle.com/datasets/nexuswho/laboro-tomato
  - 노트북에서 6클래스를 숙도 3클래스로 재매핑.
- PlantSeg Tomato subset (LabelMe polygons): https://zenodo.org/records/13762907
  - 토마토 질병 7클래스(예: bacterial_leaf_spot, early_blight, late_blight 등).

## 🔥 모델/기술

- YOLOv8m-seg: 숙도 Segmentation 학습.
- YOLOv11m-seg: 질병 Segmentation 학습.
- SegNeXt(MMSegmentation): 질병 Segmentation 대안.
- SAM2: 질병 경계 정밀 보정.
- Streamlit + ngrok: TomatoDoctor 웹 UI.

## 🛠️ 파이프라인

1. COCO/LabelMe 데이터를 YOLO/MMSegmentation 형식으로 변환.
2. 숙도 모델(YOLOv8m-seg)과 질병 모델(YOLOv11m-seg 또는 SegNeXt)을 독립 학습.
3. 추론 시 숙도 + 질병 결과를 하나의 이미지로 오버레이.
4. 샘플 결과 이미지를 `result/`에 저장.
5. Streamlit 앱에서 통합 진단 UI 제공(세션마다 URL 변경).

## 📂 레포 구조

```text
.
├── notebook/
│   ├── Laboro_yolov8m.ipynb
│   ├── plantseg_yolov11.ipynb
│   ├── SegNeXt(MMSegmentation).ipynb
│   ├── Tomato_Final(Lyolo+Pyolo).ipynb
│   └── TomatoDoctor(Lyolo+Pseg).ipynb
├── result/
│   ├── Doctor_jpg/
│   │   ├── detection.png
│   │   └── main.png
│   └── Final_jpg/
│       ├── IMG_0986_analyzed.jpg
│       ├── IMG_0987_analyzed.jpg
│       ├── IMG_0988_analyzed.jpg
│       └── tomato_early_blight_1_analyzed.jpg
├── LICENSE
└── README.md
```

## 📒 노트북 요약

| Notebook | 주요 역할 | 산출물 |
| :--- | :--- | :--- |
| **`Laboro_yolov8m.ipynb`** | **[숙도 학습]** 토마토 숙도 3단계 분류 (YOLOv8m) | 숙도 탐지 모델 (`.pt`) |
| **`plantseg_yolov11.ipynb`** | **[질병 학습 1]** 질병 탐지 및 SAM2 연동 (YOLOv11m) | 질병 탐지 모델 (`.pt`) |
| **`SegNeXt(MMSegmentation).ipynb`** | **[질병 학습 2]** 정밀 분할을 위한 심화 모델 학습 (SegNeXt) | 고성능 분할 모델 (`.pth`) |
| **`Tomato_Final(Lyolo+Pyolo).ipynb`** | **[통합 추론]** 숙도 + 질병 모델 + SAM2 결합 및 시각화 | 최종 분석 이미지 (Overlay) |
| **`TomatoDoctor(Lyolo+Pseg).ipynb`** | **[웹 서비스]** Streamlit UI 구동 및 Gemini 진단 리포트 생성 | AI 진단 대시보드 URL |

## 🍅 결과

- 오버레이 PNG 결과는 `result/`에 저장됩니다.
- 동일 결과의 JPG는 `result/jpg/`에 보관됩니다.
- streamlit의 url 결과로 gemini의 안내를 받을 수 있습니다.
