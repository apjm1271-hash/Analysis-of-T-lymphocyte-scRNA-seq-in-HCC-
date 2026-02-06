# Analysis-of-T-lymphocyte-scRNA-seq-in-HCC-

# GSE140228 데이터셋을 이용한 간암(HCC) T cell Exhaustion 분석

## 📊 프로젝트 개요 (Project Overview)
본 프로젝트는 **간세포암(HCC, Hepatocellular Carcinoma)** 환자의 scRNA-seq 데이터(GSE140228)를 분석하여, 종양 미세환경(TME) 내 **T 세포의 탈진(Exhaustion) 기전**을 규명하는 것을 목표로 합니다.
Scanpy를 활용한 전처리 파이프라인을 구축하고, Normal 조직과 Tumor 조직 간의 T 세포 유전자 발현 패턴을 비교 분석하였습니다.

## 🛠 사용 기술 (Tech Stack)
* **Language:** Python 3.x
* **Library:** Scanpy, Pandas, Matplotlib, Seaborn
* **Data:** NCBI GEO (GSE140228)

## 🔄 분석 워크플로우 (Analysis Workflow)

전체 분석 과정은 `scanpy` 라이브러리를 기반으로 수행되었으며, 주요 단계는 다음과 같습니다.

### 1. 데이터 수집 및 품질 관리 (Data Acquisition & QC)
* **데이터 로드:** GSE140228 데이터셋 로드
* **QC 및 필터링:** `sc.pp.calculate_qc_metrics()` 활용
    * **Gene Count:** 유전자 발현량이 200개 미만인 저품질 세포 제거
    * **Mitochondrial Rate:** 미토콘드리아 유전자 비율이 20% 이상인 세포(사멸 세포) 제거

### 2. 정규화 및 변수 선택 (Normalization & Feature Selection)
* **Normalization:** 세포 간 시퀀싱 깊이(Depth) 차이를 보정하기 위해 총 Count 정규화 수행
* **Log Transformation:** 데이터 분포 보정을 위한 Log1p 변환
* **HVG Selection:** 생물학적 변동성이 큰 **상위 2,000개 유전자(Highly Variable Genes)** 선별
* **Scaling:** 유전자 발현 스케일 조정 (Z-score normalization)

### 3. 차원 축소 및 군집화 (Dimensionality Reduction & Clustering)
* **PCA:** 2,000개 고변동성 유전자를 30-50개의 주성분(PC)으로 압축
* **Neighborhood Graph:** PCA 공간 상에서 세포 간 근접 이웃 계산
* **UMAP:** 고차원 데이터를 2차원으로 시각화하여 세포 분포 확인
* **Clustering:** Leiden 알고리즘을 적용하여 세포 군집(Cluster) 분류

### 4. 노이즈 제거 및 데이터 정제 (Refinement)
* **문제 발견:** T cell 군집 내에서 B cell/Plasma cell의 마커인 **Ig 유전자(IGHG1, IGKC 등)**가 높게 발현되는 현상 관찰
* **가설 및 검증:** T 세포는 항체를 생성하지 않으므로, 이는 종양 조직 내 괴사된 Plasma cell로부터 유래한 **Ambient RNA Contamination(오염)**으로 판단
* **조치:** Ig 관련 유전자 및 불필요한 Noise 유전자를 제거한 후 재분석 수행하여 데이터 신뢰도 확보

### 5. 생물학적 해석 (Biological Interpretation)
* **초기 가설:** Tumor 조직의 T cell은 Normal 조직에 비해 Exhaustion Marker가 높고 Activation Marker는 낮을 것으로 예상
* **분석 결과:** Tumor 내 T cell에서 **Activation Marker(CD52 등)**와 **Exhaustion Marker(DUSP4 등)**가 **동시에 높게 발현**되는 패턴 확인
* **결
