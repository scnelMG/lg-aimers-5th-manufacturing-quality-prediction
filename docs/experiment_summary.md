# 실험 요약

## 대표 노트북 선정

`code.ipynb`를 `notebooks/final_modeling.ipynb`로 복사해 대표 노트북으로 정리했습니다. 같은 계열의 최신 실험본인 `점수_젤_높은거_최근.ipynb`는 Optuna, CatBoost, SHAP 등 추가 실험이 섞여 있어 보조 실험 자료로 분리했습니다.

## 실험 흐름

1. 데이터 확인
   - 학습 데이터와 테스트 데이터의 행/열 수 확인
   - 타깃 비율 확인으로 클래스 불균형 파악
   - 테스트 데이터의 `Set ID`와 제출용 `target` 컬럼 구조 확인

2. 전처리
   - 전체 결측 컬럼 제거
   - 단일 값만 가진 컬럼 제거
   - 불필요한 판정값 컬럼과 중복성 높은 컬럼 제거
   - 범주형 컬럼 인코딩

3. 상관관계 기반 피처 처리
   - 상관계수 1.0인 피처 그룹 확인
   - 상관계수 절댓값 0.99 이상인 피처 쌍 확인
   - 유사한 공정 피처를 `SUM`, `DELTA` 파생변수로 변환

4. 모델링
   - RandomForest를 기준 모델로 사용
   - XGBoost, LightGBM, CatBoost를 후보 모델로 비교
   - Stratified K-Fold로 검증 데이터의 타깃 비율을 유지

5. 불균형 대응
   - Normal/AbNormal 비율 조정을 위한 언더샘플링 실험
   - RandomOverSampler를 활용한 오버샘플링 실험
   - F1 점수 기준 임계값 탐색

6. 제출 파일 생성
   - 전처리된 테스트 데이터에 최종 모델 예측 적용
   - 예측값을 `Normal` / `AbNormal` 라벨로 변환
   - 대회 제공 `submission.csv` 템플릿에 결과 저장

## 보조 노트북

- `notebooks/experiments/latest_score_experiment.ipynb`: 최신 실험 및 추가 모델링 시도
- `notebooks/experiments/feature_importance_experiment.ipynb`: 피처 중요도와 오토인코더 기반 이상 패턴 탐색
- `notebooks/experiments/process_analysis_experiment.ipynb`: 공정별 피처 구조와 상관관계 분석
