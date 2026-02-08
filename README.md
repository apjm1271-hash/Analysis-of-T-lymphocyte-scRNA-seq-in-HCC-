# Analysis-of-T-lymphocyte-scRNA-seq-in-HCC-


# Analysis of T-lymphocytes scRNA-seq in HCC
**GSE140228 데이터셋을 이용한 간세포암(HCC) 내 T cell scRNA-seq 분석**

본 프로젝트는 **Python**과 **Scanpy**를 활용하여, 간세포암(HCC) 환자의 Normal 조직과 Tumor 조직 내 T cell의 전사체 데이터를 비교 분석한 연구입니다.

## 1. 프로젝트 개요 (Overview)
* **연구 목표:** Normal 조직과 Tumor 조직의 T cell을 비교 분석하여, "종양 조직 내 T cell에서 Exhaustion marker들이 많이 발현될 것"이라는 가설을 검증하고 그 특성을 규명함.
* **데이터셋:** [GSE140228](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE140228) (10x Genomics scRNA-seq).
* **주요 발견:** 종양 조직 내 T cell은 단순히 소진(Exhaustion)된 상태가 아니라, **활성화(Activation)와 소진(Exhaustion) 마커가 동시에 높게 발현**되는 경향을 보임.

## 2. 분석 워크플로우 (Workflow)

분석은 크게 데이터 수집, 전처리, 분석 단계로 진행되었습니다.

### A. 데이터 전처리 (Preprocessing)
수집된 Raw 데이터의 품질 관리(QC) 및 노이즈 제거를 수행했습니다.
* **QC 및 필터링:** `sc.pp.calculate_qc_metrics()` 사용.
    * 유전자 수 200개 미만인 저품질 세포 제거.
    * 미토콘드리아(Mitochondrial) mRNA 비율이 20% 이상인, 손상된 세포(Dying cells) 제거.
* **정규화 및 차원 축소:**
    * 세포 간 Read count 차이 보정 및 Z-normalization 수행.
    * 상위 2,000개 고변동성 유전자(Highly Variable Genes) 선별.
    * PCA(주성분 분석)를 통해 30~50개 차원으로 압축 후 이웃 그래프 생성.

### B. 군집화 및 시각화 (Clustering & Visualization)
* **UMAP:** 세포들의 유전자 발현 패턴 유사도를 기반으로 2차원 시각화.
<img width="1945" height="429" alt="image" src="https://github.com/user-attachments/assets/541fc539-b6b3-4367-918e-46127c4d0c14" />

* **해석:** 세포들은 Cell Type에 따라 뚜렷하게 군집을 형성하나, 동일 군집 내에서 Tumor와 Normal 세포가 혼재되어 나타남. 이는 종양 미세환경(TME)이 존재하더라도 면역세포 고유의 Lineage 특성은 유지됨을 시사함.
<img width="1361" height="429" alt="image" src="https://github.com/user-attachments/assets/238ca5fc-b976-48a1-a0e3-72e63f774046" />


### C. DEG (차별 발현 유전자) 분석
* **노이즈 제어:** 초기 분석 시 T cell에서 `IG-`(Immunoglobulin) 유전자가 높게 검출됨. 이는 Tumor 조직 내 괴사(Necrosis)된 Plasma cell에서 유래한 오염으로 판단하여 해당 유전자들을 제거 후 재분석 수행.
<img width="331" height="259" alt="image" src="https://github.com/user-attachments/assets/0f29be94-a0f4-4216-ada3-61ef2cdc219a" />
<img width="870" height="231" alt="image" src="https://github.com/user-attachments/assets/b9c3cc6e-9217-4f86-b00a-e452e39a7c29" />

## 3. 주요 분석 결과 (Key Findings)
<img width="331" height="269" alt="image" src="https://github.com/user-attachments/assets/00bf380d-d1c6-489b-a7c3-3dfef8a6c87a" />
<img width="880" height="231" alt="image" src="https://github.com/user-attachments/assets/9f5538ee-ecf4-4cbe-b78d-89349df6a487" />
<img width="563" height="453" alt="image" src="https://github.com/user-attachments/assets/f1f13fff-a133-4917-af9f-c3e51b8615f1" />

Tumor 조직 내 T cell에서 가장 유의미하게 변화된 **Top 5 유전자**와 그 생물학적 의미는 다음과 같습니다.

| 유전자 (Gene) | 기능 및 해석 (Interpretation) |
|:---:|:---|
| **CD52** | T cell 활성화 및 잠재적 소진 유도(Soluble form) |
| **GAPDH** | Glycolysis 효소, T cell effector function 증가와 관련 |
| **TMSB10** | 세포 골격 재구성(Cytoskeleton remodeling), 종양 침투 및 이동성 마커 |
| **COTL1** | F-actin 안정화, 면역 세포 침투(Immune infiltration)와 양의 상관관계 |
| **DUSP4** | MAPK 경로 억제(Negative feedback), **T cell Exhaustion 유발의 핵심 인자** |

## 4. 고찰 및 결론 (Discussion)

* **가설 검증:** 초기 가설과 달리, 종양 내 T cell에서는 Exhaustion marker(DUSP4 등)뿐만 아니라 Activation marker도 함께 Upregulation 되는 현상이 관찰됨.
* **결론:** T cell의 소진(Exhaustion)은 활성화(Activation)와 대립되는 별개의 상태가 아님. **"지속적인 자극에 의해 활성화된 T cell이 점진적으로 기능을 상실해가는 연속적인 과정(Spectrum)"**으로 이해해야 함.
* **한계점:** scRNA-seq 데이터만으로는 CD52와 같은 유전자의 정확한 형태(Membrane-bound vs Soluble)를 구별하는 데 한계가 있어 추가 검증이 필요함.

## 5. 추후 연구 계획 (Future Work)
본 연구 결과를 바탕으로 **염증성 종양 미세환경(Inflammatory TME)**과 T cell 소진의 인과관계를 규명하는 후속 연구를 계획 중입니다.
* **핵심 질문:** TME의 만성적인 염증 반응이 실제로 T cell exhaustion을 유발하는가?
* **계획:** 사이토카인 등 염증 관련 유전자군과 Exhaustion marker 간의 발현 상관관계 분석.

## 6. 사용 언어 및 라이브러리 (Dependencies)
* **Python**
* **Scanpy**
* Matplotlib / Pandas / NumPy

---
*Created by [Jeongmin Park]. Department of Biomedical Engineering, Jeonbuk National University.*
