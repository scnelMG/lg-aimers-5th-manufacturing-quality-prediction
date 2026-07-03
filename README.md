# LG AImers 5기 - 제조 품질 이상 탐지

> 고차원 제조 공정 데이터에서 `Normal` / `AbNormal`을 예측하는 tabular ML 경진대회 프로젝트입니다.

[![Python](https://img.shields.io/badge/Python-ML%20Pipeline-3776AB?logo=python&logoColor=white)](requirements.txt)
[![LightGBM](https://img.shields.io/badge/LightGBM-Tabular%20ML-02569B)](docs/experiment_summary.md)
[![XGBoost](https://img.shields.io/badge/XGBoost-Imbalanced%20Classification-FF6600)](docs/reproducibility.md)
[![Portfolio](https://img.shields.io/badge/Portfolio-Competition%20ML-2ea44f)](README.md)

## 개요

LG AImers 5기 온라인 해커톤에서 진행한 제조 품질 예측 프로젝트입니다. 공정, 장비, 검사, 작업 지시 관련 고차원 feature를 사용해 제조 결과가 정상인지 비정상인지 분류하는 문제를 다뤘습니다.

이 저장소는 원본 대회 데이터를 포함하지 않고, 공개 가능한 notebook, 실험 요약, 재현 가이드, 데이터 공개 정책을 중심으로 정리한 포트폴리오 버전입니다.

## 빠른 검토 경로

| 먼저 볼 것 | 확인할 내용 |
| --- | --- |
| [docs/experiment_summary.md](docs/experiment_summary.md) | 전처리, feature engineering, 모델 비교, threshold 검토 |
| [docs/reproducibility.md](docs/reproducibility.md) | 대회 데이터 없이 확인 가능한 범위와 실행 조건 |
| [notebooks/](notebooks/) | 원본 실험 흐름과 제출 파일 생성 과정 |
| [requirements.txt](requirements.txt) | 분석 환경 구성 |

## 문제 정의

입력 테이블은 460개 이상의 제조 공정 feature와 약 4만 개 row를 포함합니다. 결측, 상수 컬럼, 중복/유사 공정 변수, 범주형 feature, 강한 클래스 불균형이 함께 존재하기 때문에 단순 정확도보다 `AbNormal` class를 얼마나 안정적으로 잡는지가 중요했습니다.

## 내 역할

- 완전 결측, 상수, 중복/노이즈성 컬럼 제거
- 반복 공정 feature에 대한 `SUM`, `DELTA`, correlation group 기반 feature engineering
- Stratified validation 기반 tree model 비교
- `AbNormal` 소수 class를 고려한 oversampling, undersampling, threshold 검토
- 실험 요약과 공개 가능한 재현 문서 정리

## 기술적 의사결정

| 영역 | 선택 | 이유 |
| --- | --- | --- |
| 모델군 | LightGBM, XGBoost, CatBoost | 고차원 tabular 데이터와 결측/범주형 feature 처리에 강합니다. |
| 검증 | Stratified split | 소수 class 비율이 validation에 유지되도록 했습니다. |
| 불균형 대응 | RandomOverSampler, class weight, threshold review | F1 중심의 trade-off를 보기 위한 구성입니다. |
| feature engineering | 상수/결측 제거, correlation group, 파생 feature | 노이즈가 많은 제조 feature에서 학습 안정성을 높이기 위한 선택입니다. |

## 파이프라인

```mermaid
flowchart LR
    A["대회 CSV"] --> B["결측/상수/중복 컬럼 정리"]
    B --> C["공정 feature engineering"]
    C --> D["불균형 처리"]
    D --> E["Tree model 비교"]
    E --> F["Threshold 검토"]
    F --> G["Submission 생성"]
```

## 결과 근거

이 저장소는 leaderboard claim보다 실험 판단 근거를 보여주는 데 초점을 둡니다.

- 고차원 제조 feature 정리 전략
- 소수 class F1을 중심으로 한 검증 방식
- 모델별 precision/recall/F1 trade-off 검토
- 대회 데이터 미포함 상태에서도 읽을 수 있는 notebook과 실험 요약

## 재현 가능성

```bash
pip install -r requirements.txt
```

전체 실행에는 LG AImers/DACON에서 제공한 공식 CSV가 필요합니다. 공개 저장소에는 원본 대회 데이터와 제출 CSV를 포함하지 않습니다.

## 공개/비공개 경계

포함:

- 공개 가능한 notebook과 실험 요약
- 실행 환경과 재현 가이드
- 모델링 의사결정 문서

제외:

- 원본 대회 데이터
- 제출 파일, 개인 인증서, scratch notebook
- 대용량 artifact, Drive archive, 개인정보 가능 자료

## 한계

- 대회 데이터가 없으면 end-to-end 재현은 불가능합니다.
- 제조 현장 deployment, 모니터링, drift 대응은 포함하지 않았습니다.
- class imbalance가 큰 문제이므로 단일 metric만으로 모델을 평가할 수 없습니다.
