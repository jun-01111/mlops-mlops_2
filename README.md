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
```

## Docker를 이용한 실행 방법

이 프로젝트는 Docker를 사용하여 환경에 구애받지 않고 일관된 학습 파이프라인을 실행할 수 있도록 구성되어 있습니다.

### 1. 사전 준비 (Prerequisites)

프로젝트 실행 전, 보안 및 설정을 위해 다음 파일들이 로컬에 준비되어야 합니다.
* **`.env` 파일 생성**: 프로젝트 루트 디렉토리에 `.env.template` 파일을 복사하여 `.env` 파일을 만듭니다.
    ```bash
    cp .env.template .env
    ```
* **환경 변수 설정**: 생성한 `.env` 파일을 열어 다음 정보를 입력합니다.
    ```
    1. WANDB 설정
    # Weights & Biases 대시보드 접속을 위한 API 키
    WANDB_API_KEY=your_wandb_api_key_here
    
    # 2. TMDB API 설정
    # 영화 데이터 수집을 위한 TMDB API Read Access Token (또는 API Key)
    TMDB_API_KEY=your_tmdb_api_key_here
    
    # 3. AWS 자격 증명 및 S3 설정
    # S3 버킷 접근 및 모델 업로드를 위한 권한
    AWS_ACCESS_KEY_ID=your_access_key_id_here
    AWS_SECRET_ACCESS_KEY=your_secret_access_key_here
    AWS_REGION=ap-northeast-2
    S3_BUCKET=your_s3_bucket_name_here
    ```

### 2. 도커 이미지 빌드 (Build)

Dockerfile이 위치한 디렉토리에서 아래 명령어를 입력하여 이미지를 빌드합니다.

```bash
docker build -t tmdb-pipeline:latest .
```

### 3.실행 가이드 (Execution Guide)

이 프로젝트는 `main.py`를 통해 모든 파이프라인을 제어하며, Docker 컨테이너 환경 또는 로컬 CLI 환경에서 실행할 수 있습니다.

#### 전체 파이프라인 실행 (collect -> preprocess -> train)
```
docker run --rm --env-file .env tmdb-pipeline:latest run_all
```

#### 특정 단계만 실행 (예: 전처리만 실행)
```
docker run --rm --env-file .env tmdb-pipeline:latest preprocess
```

#### 컨테이너 내부 접속 후 CLI 실행

컨테이너를 백그라운드에서 실행(`-d`)한 뒤, 내부 쉘에 접속하여 직접 `main.py`의 다양한 명령어를 테스트할 수 있습니다.

#### **1) 컨테이너 백그라운드 실행**
```bash
docker run -itd --name tmdb_test --env-file .env tmdb-pipeline:latest /bin/bash
```
#### **2) 실행 중인 컨테이너 내부 접속
```
docker exec -it tmdb_test bash
```
#### **3) 내부에서 CLI 명령어 실행
```
python main.py collect
python main.py preprocess
python main.py run_all
```


