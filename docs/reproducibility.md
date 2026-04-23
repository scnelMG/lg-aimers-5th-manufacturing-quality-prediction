# 재현 방법

## 환경

권장 Python 버전은 3.9 이상입니다. 대표 노트북은 Jupyter Notebook 또는 JupyterLab에서 실행할 수 있습니다.

```bash
pip install -r requirements.txt
```

## 데이터 배치

원본 데이터는 저장소에 포함하지 않습니다. 노트북을 실행하려면 대회에서 제공된 데이터를 아래 경로에 직접 배치해야 합니다.

```text
data/
  train.csv
  test.csv
  submission.csv
```

## 실행 순서

1. 의존성 설치
2. `data/` 디렉터리에 원본 CSV 배치
3. `notebooks/final_modeling.ipynb` 실행
4. 전처리, 모델 학습, 검증, 예측 순서로 셀 실행
5. 제출 파일 생성 셀에서 `submission.csv` 템플릿에 예측 결과 저장

## 주의 사항

- 원본 데이터와 제출 CSV는 `.gitignore`로 제외되어 있습니다.
- 일부 실험 노트북은 Colab 환경에서 작성된 셀이나 추가 패키지 설치 셀을 포함할 수 있습니다.
- 검증 점수와 최종 제출 점수는 실행 환경, 라이브러리 버전, 샘플링 랜덤 시드에 따라 달라질 수 있습니다.
