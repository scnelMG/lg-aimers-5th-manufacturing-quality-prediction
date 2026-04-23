# LG Aimers 5기 온라인 해커톤 - 제조 품질 이상 탐지

제조 공정 데이터를 활용해 제품의 정상/이상 여부를 예측한 이진 분류 프로젝트입니다. 약 4만 건의 학습 데이터와 460개 이상의 공정 피처를 바탕으로 `Normal` / `AbNormal`을 분류하고, 클래스 불균형 상황에서 F1 성능을 개선하는 데 초점을 맞췄습니다.

## 프로젝트 개요

- 주제: 제조 공정 품질 이상 여부 예측
- 문제 유형: 이진 분류
- 타깃: `Normal`, `AbNormal`
- 주요 과제: 고차원 공정 피처, 결측/상수 컬럼, 강한 피처 상관관계, 클래스 불균형
- 대표 노트북: [`notebooks/final_modeling.ipynb`](notebooks/final_modeling.ipynb)

## 데이터

원본 데이터는 LG Aimers 온라인 해커톤에서 제공된 대회 데이터입니다. 데이터 공개 범위와 라이선스를 고려해 저장소에는 원본 `train.csv`, `test.csv`, `submission.csv` 및 제출용 CSV 파일을 포함하지 않았습니다.

노트북 실행 시에는 아래 구조로 원본 데이터를 별도 배치해야 합니다.

```text
data/
  train.csv
  test.csv
  submission.csv
```

## 접근 방법

1. 데이터 구조 파악
   - `train.csv`: 약 40,506행, 464개 컬럼
   - `test.csv`: 약 17,361행, `Set ID`와 제출용 `target` 컬럼 포함
   - 공정별 반복 컬럼과 수치/범주형 피처가 혼재

2. 전처리
   - 전체 결측 컬럼, 단일 값 컬럼, 중복성 높은 컬럼 제거
   - `OK` 등 판정값이 섞인 컬럼 정리
   - 공정별 반복 피처 구조 확인
   - `LabelEncoder`, `OneHotEncoder` 기반 범주형 인코딩

3. 피처 엔지니어링
   - 상관관계가 1.0 또는 0.99 이상인 피처 그룹 탐색
   - 유사 공정 피처를 활용한 `SUM`, `DELTA` 파생변수 생성
   - 불필요하거나 정보 중복이 큰 피처 제거

4. 모델링 및 검증
   - RandomForest, XGBoost, LightGBM, CatBoost 계열 모델 실험
   - Stratified K-Fold 기반 검증
   - 언더샘플링과 RandomOverSampler로 클래스 불균형 대응
   - F1 기준 예측 임계값 탐색

## 저장소 구성

```text
.
├── README.md
├── requirements.txt
├── notebooks/
│   ├── final_modeling.ipynb
│   └── experiments/
│       ├── latest_score_experiment.ipynb
│       ├── feature_importance_experiment.ipynb
│       └── process_analysis_experiment.ipynb
├── docs/
│   ├── experiment_summary.md
│   └── reproducibility.md
└── assets/
    ├── decision_tree_high_res.png
    ├── model_output.png
    ├── presentation_lg_aimers_5th_online_hackathon.pptx
    └── lg_ai_certificate.pdf
```

## 주요 산출물

- [`notebooks/final_modeling.ipynb`](notebooks/final_modeling.ipynb): 최종 포트폴리오용 분석 및 모델링 노트북
- [`docs/experiment_summary.md`](docs/experiment_summary.md): 실험 흐름 요약
- [`docs/reproducibility.md`](docs/reproducibility.md): 재현 환경과 실행 절차
- [`assets/presentation_lg_aimers_5th_online_hackathon.pptx`](assets/presentation_lg_aimers_5th_online_hackathon.pptx): 프로젝트 발표 자료

## 회고

이 프로젝트에서는 단순히 모델을 바꾸는 것보다 제조 공정 데이터의 구조를 이해하고, 반복 공정 피처와 높은 상관관계를 가진 피처를 어떻게 다룰지가 중요했습니다. 특히 이상 클래스가 적은 상황에서 F1을 개선하기 위해 샘플링 전략과 임계값 조정을 함께 실험했습니다.

포트폴리오 관점에서는 고차원 테이블 데이터 전처리, 불균형 분류, 트리 기반 모델 비교, 제출 후처리까지 한 사이클을 경험한 프로젝트입니다.
