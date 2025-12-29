[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/xoFPmgXs)

# TMDB Movie Success Prediction Pipeline

TMDB(The Movie Database) API를 활용하여 인기 영화 데이터를 수집하고, 선형 회귀(Linear Regression) 모델을 통해 영화의 평점을 예측하는 End-to-End MLOps 파이프라인입니다.

이 프로젝트는 데이터 수집, 전처리, 학습, 모델 검증, 그리고 AWS S3를 활용한 모델 배포 자동화 프로세스를 포함합니다.

---

## Key Features

- **Data Collection**: TMDB API를 통해 실시간 인기 영화 메타데이터 수집
- **Preprocessing**: 범주형 데이터 인코딩 및 학습용 피처 엔지니어링 수행
- **Model Training**: Scikit-learn 기반의 선형 회귀 모델 학습
- **Champion/Challenger Strategy**: 기존 모델(Champion)과 신규 모델(Challenger)의 성능(MSE)을 비교하여 우수한 모델을 자동 교체 및 배포
- **AWS S3 Integration**: 학습된 모델 및 평가지표(Metrics)를 날짜별로 클라우드에 아카이빙
- **Containerization**: Docker를 활용하여 환경에 구애받지 않는 일관된 실행 환경 제공

---

## Project Structure

```text
.
├── core/
│   ├── config.py          # 환경 변수 및 설정 관리
│   └── s3_client.py       # AWS S3 업로드 및 다운로드 엔진
├── src/
│   ├── collector.py       # TMDB API 데이터 수집기
│   ├── preprocessor.py    # 데이터 정제 및 전처리 로직
│   └── train.py           # 모델 학습 및 성능 비교 로직
├── main.py                # 전체 파이프라인 실행 엔트리포인트
├── Dockerfile             # 이미지 빌드 설계도
├── requirements.txt       # 의존성 패키지 목록
└── .env                   # API 키 및 인증 정보 (Git 제외 대상)
