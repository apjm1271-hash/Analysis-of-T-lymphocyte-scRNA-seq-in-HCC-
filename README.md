# Analysis-of-T-lymphocyte-scRNA-seq-in-HCC-

# scRNA-seq Analysis of T Cell Exhaustion in HCC (GSE140228)

## 📊 Project Overview
This project analyzes single-cell RNA sequencing (scRNA-seq) data from Hepatocellular Carcinoma (HCC) patients to investigate T cell exhaustion mechanisms within the Tumor Microenvironment (TME). Using the **GSE140228** dataset, we performed clustering, visualization, and Differential Expression Gene (DEG) analysis to compare T cell states in Normal vs. Tumor tissues.

## 🛠 Tech Stack
* **Language:** Python
* **Library:** Scanpy, Matplotlib, Seaborn, Pandas

## 🔄 Analysis Workflow

The analysis pipeline was constructed using `scanpy` with the following key steps:

### 1. Data Acquisition & Preprocessing
* [cite_start]**Data Source:** GSE140228 (NCBI GEO) [cite: 24, 25]
* **Quality Control (QC):**
    * [cite_start]Calculated QC metrics using `sc.pp.calculate_qc_metrics()`[cite: 27].
    * **Filtering:** Removed low-quality cells based on the following criteria:
        * [cite_start]Cells with < 200 expressed genes (fragments)[cite: 28].
        * [cite_start]Cells with > 20% mitochondrial mRNA content (indicative of dying cells)[cite: 28].

### 2. Normalization & Feature Selection
* [cite_start]**Normalization:** Normalized counts to total counts per cell to correct for sequencing depth differences[cite: 29].
* **Log Transformation:** Applied log1p transformation.
* [cite_start]**Feature Selection:** Identified the top **2,000 highly variable genes (HVGs)** to capture biological variability[cite: 30].
* [cite_start]**Scaling:** Performed Z-score normalization (`scale`) to adjust gene expression scales[cite: 32].

### 3. Dimensionality Reduction & Clustering
* [cite_start]**PCA:** Compressed data into 30-50 Principal Components (PCs)[cite: 33].
* [cite_start]**Neighborhood Graph:** Computed nearest neighbors in the PCA space[cite: 34].
* [cite_start]**UMAP:** Visualized high-dimensional data in 2D space based on gene expression patterns.
* **Clustering:** Applied Leiden clustering algorithm for cell type identification.

### 4. Downstream Analysis & Noise Removal (Refinement)
* [cite_start]**Hypothesis:** T cells in tumor tissue are expected to show higher expression of exhaustion markers compared to normal tissue[cite: 44].
* **Initial DEG Analysis:** Identified Differentially Expressed Genes (DEGs) between Normal and Tumor tissues.
* **Noise Filtering:**
    * [cite_start]Observed high expression of Immunoglobulin (Ig) genes (e.g., *IGHG1, IGKC*) in T cell clusters[cite: 46].
    * [cite_start]**Interpretation:** T cells do not produce Ig; this was identified as contamination from necrotic plasma cells in the tumor tissue.
    * [cite_start]**Action:** Removed noise genes (IG-, MT- prefixes) and re-ran the DEG analysis for accurate interpretation[cite: 49].

### 5. Biological Interpretation
* [cite_start]Analyzed key markers: **CD52, GAPDH, TMSB10, COTL1, DUSP4**[cite: 50, 57].
* [cite_start]**Key Finding:** Tumor-infiltrating T cells showed high expression of both Activation markers (*CD52*) and Exhaustion markers (*DUSP4*), suggesting that exhaustion is a progressive state following chronic activation rather than an independent phenotype[cite: 78, 80].
