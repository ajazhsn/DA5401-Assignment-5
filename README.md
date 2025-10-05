# 🧬 Dimensionality Reduction and Visualization of Gene Expression Data

This project explores the use of **t-SNE** and **Isomap** for visualizing high-dimensional **gene expression data**.  
Through preprocessing, dimensionality reduction, and visual interpretation, we aim to uncover underlying data structures, detect anomalies, and compare how different manifold learning methods capture relationships between samples.

---

## 📁 Overview

| Part | Topic | Focus |
|------|--------|--------|
| **Part A** | Preprocessing & Initial Setup | Data loading, dimensionality checks, label simplification, and feature scaling |
| **Part B** | t-SNE & Veracity Inspection | Local structure visualization, perplexity analysis, and qualitative inspection |
| **Part C** | Isomap & Manifold Learning | Global structure visualization, neighbor parameter tuning, and curvature comparison |

---

## 🧩 Part A: Preprocessing and Initial Setup

### 1️⃣ Data Loading
- Loaded **feature matrix (X)** with 86 features and **multi-label target matrix (Y)** with 14 labels.
- Verified dataset shape and ensured consistency across samples.

### 2️⃣ Dimensionality Check
- Reported the **initial dimensionality** of the dataset, confirming 86-dimensional feature space and 14 class labels.

### 3️⃣ Label Selection for Visualization
- Simplified the multi-label problem:
  - Selected the **most frequent single-label** and the **most frequent multi-label combination**.
  - Grouped all other labels as **“Other”**.
- This yielded a **3-class scheme**:  
  `Top_Single_Label (Blue)`, `Top_Multi_Label_Combo (Red)`, and `Other (Light Grey)`.

### 4️⃣ Scaling (Standardization)
- Applied **StandardScaler** to normalize all features to zero mean and unit variance.
- Standardization is crucial for distance-based methods (like t-SNE and Isomap) to ensure fair feature contribution.

---

## 🌈 Part B: t-SNE and Veracity Inspection

### 🧠 1. t-SNE Implementation
- Applied **t-distributed Stochastic Neighbor Embedding (t-SNE)** to reduce data from 86D → 2D.
- Experimented with **perplexity values**: 5, 20, 30, 50.
- For each perplexity:
  - Evaluated **Trustworthiness**, **Continuity**, and **Kullback–Leibler Divergence (KLD)**.
  - Identified **Perplexity = 20** as the most balanced choice (high trust, moderate continuity, stable KLD).

### 🎨 2. Visualization
- Generated scatter plots where:
  - Blue points = Top single-label samples  
  - Red points = Most common multi-label combination  
  - Grey points = Remaining “Other” samples  
- Observed local clusters with varying degrees of overlap depending on perplexity.

### 🔍 3. Veracity Inspection
Performed a visual quality assessment:

- **🧩 Noisy / Ambiguous Labels:**  
  Red or blue points embedded in dense grey clusters, indicating possible label noise or overlap.

- **📍 Outliers:**  
  Sparse, isolated points located far from main clusters — potential anomalies or rare gene patterns.

- **⚖️ Hard-to-Learn Samples:**  
  Mixed regions with multiple colors — suggest nonlinear relationships where simple classifiers may fail.

---

## 🌐 Part C: Isomap and Manifold Learning

### 🧭 1. Isomap Implementation
- Applied **Isomap** for nonlinear dimensionality reduction while preserving **global geometric structure**.
- Tested various neighborhood sizes: `n_neighbors = [5, 10, 20, 30, 50]`.

### 📊 2. Evaluation Metrics
For each neighborhood size, measured:
- **Trustworthiness** — local neighborhood preservation  
- **Continuity** — global consistency  
- **Reconstruction Error** — how well the original distances are preserved  

| Neighbors | Trustworthiness | Continuity | Reconstruction Error |
|------------|-----------------|-------------|------------------------|
| 5 | 0.732 | 0.025 | 290.32 |
| 10 | 0.732 | 0.025 | 189.94 |
| 20 | 0.732 | 0.025 | 142.10 |
| 30 | 0.732 | 0.025 | 119.90 |
| 50 | 0.732 | 0.025 | **108.90** |

➡️ **Best Isomap:** `n_neighbors = 50` (lowest reconstruction error).

### 🎨 3. Visualization
- Created 2D Isomap scatter plots using the same color scheme as t-SNE.
- Observed smoother global transitions between clusters.

### 🧩 4. Comparison and Curvature
- **Global vs. Local Preservation:**  
  - **t-SNE** captures **local cluster structure** effectively.  
  - **Isomap** better preserves **global geometry**, revealing continuous manifold shapes.
- **Manifold Complexity:**  
  - The Isomap plots suggest a **curved, non-linear manifold**.  
  - This complexity increases classification difficulty, especially for linear models.

---

## 🧭 Comparison Summary

| Method | Preserves | Best Param | Strength | Limitation |
|---------|------------|-------------|------------|-------------|
| **t-SNE** | Local structure | Perplexity = 20 | Excellent at revealing fine-grained clusters | Loses global distances |
| **Isomap** | Global structure | n_neighbors = 50 | Captures smooth manifold geometry | Sensitive to neighborhood choice |

---

## 🏁 Conclusion

- **t-SNE** provides more interpretable **local groupings** of similar gene expressions.  
- **Isomap** excels in preserving **global data topology** and reveals smoother transitions.  
- Combining insights from both methods offers a **comprehensive understanding** of the dataset’s structure — both at local and global levels.  
- The veracity inspection further identifies potential noise, outliers, and hard-to-learn regions, enhancing interpretability for downstream tasks.

---

## ⚙️ Requirements

```bash
python >= 3.9
numpy
pandas
matplotlib
scikit-learn
