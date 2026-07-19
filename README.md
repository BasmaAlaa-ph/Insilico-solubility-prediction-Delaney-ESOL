# In-Silico Solubility Prediction Using the Delaney ESOL Model with Pandas, Matplotlib, and Seaborn

A cheminformatics analysis pipeline that reimplements Delaney's ESOL (Estimated SOLubility) model from scratch, predicting aqueous solubility for **1,128 pharmaceutical compounds** directly from their molecular descriptors — no laboratory experiments required.

## 🧪 The Science

Aqueous solubility is one of the earliest checkpoints in oral drug development: a compound that doesn't dissolve well in water struggles to be absorbed, regardless of its other properties. Delaney's 2004 ESOL model offers a computational shortcut, estimating solubility directly from four structural descriptors:

**Log(Solubility) = 0.16 − (0.63 × cLogP) − (0.0062 × MW) + (0.066 × RB) − (0.74 × AP)**

* **cLogP:** Lipophilicity — how "water-repelling" the molecule is.
* **MW (Molecular Weight):** Larger molecules are generally harder to dissolve.
* **RB (Rotatable Bonds):** Molecular flexibility.
* **AP (Aromatic Proportion):** Fraction of the molecule that is aromatic/ring-like.

## 📊 Pipeline Architecture & Logic Steps

### 1️⃣ Data Inspection 
* **Auditing:** Inspected the dataset using `df.shape` (1,128 rows, 12 columns) and `df.columns.tolist()`.

### 2️⃣ Structural Reordering & Cleaning
* **Logical Sequencing:** Repositioned `smiles` beside `Compound ID` and `measured log solubility` beside the calculated descriptor columns for direct visual comparison.
* **Missing Value Audit:** Programmatically checked every column for null values, with automatic row-dropping and a clear pass/fail print statement confirming dataset integrity.

### 3️⃣ ESOL Formula Reimplementation
* **Vectorized Computation:** Rebuilt Delaney's formula using pure Pandas vector arithmetic (no row-by-row loops), generating a custom `My_ESOL_Solubility` column across all 1,128 compounds instantly.

### 4️⃣ Solubility Classification
* **Rule-Based Labeling:** Applied `.apply(lambda)` to classify each compound as **Acceptable** (≥ -4.0) or **Poor** (< -4.0) — a standard early-stage druglikeness solubility cutoff.
* **Summary Statistics:** Used `.value_counts(normalize=True)` to calculate pass/fail distribution as both raw counts and percentages.

### 5️⃣ Data Visualization (Seaborn + Matplotlib)
* **Correlation Analysis:** Three regression scatter plots (`sns.regplot`) examining LogP, Molecular Weight, and Polar Surface Area against calculated solubility.
* **Categorical Aggregation:** Bar plot (`sns.barplot`) and violin plot (`sns.violinplot`) automatically aggregating solubility by H-Bond Donor count and Ring count.
* **Distribution & Validation:** Pie chart visualizing status distribution, plus a measured-vs-calculated scatter plot with a diagonal reference line to visually assess model accuracy.

## 📈 Screening Results & Insights

* **Acceptable Solubility:** 889 compounds (**78.81%**)
* **Poor Solubility:** 239 compounds (**21.19%**)
* **Strongest driver of poor solubility:** Calculated LogP, showing a clear negative correlation with predicted solubility — consistent with established medicinal chemistry principles.
* **Model validation:** Calculated values track closely with experimentally measured solubility, confirming the reimplemented formula behaves consistently with Delaney's original published model.

*Takeaway: Nearly 1 in 4 compounds in this screening set falls below the acceptable solubility threshold — reinforcing why solubility screening is a critical, early-stage filter in computational drug discovery pipelines.*

## 🚀 Repository Contents
* `delaney_rdkit_ready.csv`: The raw input dataset containing 1,128 compounds and their molecular descriptors.
* `ESOL_Solubility_Final_Report.csv`: The final processed output featuring calculated solubility values and pass/fail classification.
* `plot1_logp_vs_solubility.png` – `plot7_model_validation.png`: Individual high-resolution (300 dpi) visualizations of each descriptor relationship.
* `*.ipynb`: Interactive Google Colab notebook featuring the full data pipeline, scientific commentary, and live outputs.
* `*.py`: Clean, production-ready Python script version of the analysis logic.
