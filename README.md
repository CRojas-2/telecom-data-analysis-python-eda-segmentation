# 📡 EDA & Customer Segmentation — ConnectaTel

> End-to-end exploratory data analysis of a Latin American telecom company. Covers data cleaning, outlier detection, customer segmentation by usage and age, and actionable business insights using Python and Pandas.

---

## 🎯 Project Objective

Analyze the behavioral patterns of ConnectaTel's customer base to identify usage segments, detect data quality issues, and translate findings into actionable commercial recommendations — including plan restructuring and targeted retention strategies.

---

## 📁 Datasets

Three CSV files were used, all containing data up to 2024:

| File | Description | Records |
|------|-------------|---------|
| `plans.csv` | Plan catalog: price, included minutes, GB, and overage costs | 2 rows |
| `users_latam.csv` | Customer info: age, city, registration date, plan, and churn | 4,000 rows |
| `usage.csv` | Service usage detail: calls and messages per user | ~40,000 rows |

---

## 🧩 Analysis Stages

### 1. Load & Explore
- Loaded all three datasets using `pd.read_csv()`
- Reviewed shape, column types, and initial structure with `.info()` and `.head()`

### 2. Data Quality Assessment
- **Null values:** Identified missing data per column and calculated proportions
- **Invalid values / sentinels:** Detected `-999` in `age`, `?` in `city`, and structural zeros in `duration` / `length`
- **Date validation:** Detected and flagged impossible future dates (year 2026) in `reg_date`

### 3. Data Cleaning
- Replaced sentinel `-999`, `0`, and `-1` in `age` with the column median
- Replaced `?` in `city` with `pd.NA`
- Marked year-2026 registration dates as `NaT`
- Confirmed `duration` and `length` nulls as **Missing At Random (MAR)** — kept intentionally, as each field only applies to its event type (`call` or `text`)
- Dropped 50 rows with missing `date` in `usage` (0.12%) to preserve temporal integrity

### 4. Usage Aggregation per User
- Built a user-level summary table from `usage` with:
  - `cant_mensajes` — total messages sent
  - `cant_llamadas` — total calls made
  - `cant_minutos_llamada` — total call minutes
- Merged with `users` into a unified `user_profile` dataframe

### 5. Distribution Analysis & Outlier Detection
- Plotted histograms for `age`, `cant_mensajes`, `cant_llamadas`, and `cant_minutos_llamada` segmented by plan
- Used boxplots and the **IQR method** to identify and evaluate outliers
- Key finding: a cluster of Premium users consuming 100–160 minutes/month, disconnected from the rest of the base

### 6. Customer Segmentation
- **By usage level** (`grupo_uso`):
  - `Bajo consumo`: < 5 calls and < 5 messages
  - `Uso medio`: < 10 calls and < 10 messages
  - `Alto uso`: ≥ 10 in either metric
- **By age group** (`grupo_edad`):
  - `Joven`: < 30 years
  - `Adulto`: 30–59 years
  - `Adulto mayor`: ≥ 60 years

### 7. Executive Insights
- Synthesized findings into business-ready conclusions covering segment value, churn risk, outlier implications, and plan redesign recommendations

---

## ▶️ How to Run

### Option A — Google Colab (recommended)

1. Download the notebook: `File → Download → Download .ipynb`
2. Go to [colab.research.google.com](https://colab.research.google.com)
3. Click `File → Upload notebook` and select the `.ipynb` file
4. Upload the three CSV files to the Colab session storage or mount your Google Drive
5. Update the file paths in the loading cells to match your environment
6. Run all cells: `Runtime → Run all`

### Option B — Local Environment

1. Clone this repository:
   ```bash
   git clone https://github.com/CRojas-2/CRojas-2/blob/main/S7_Version_Estudiante_Project_ConnectaTel.ipynb
   cd telecom-data-analysis-python-eda-segmentation
   ```
2. Install dependencies:
   ```bash
   pip install pandas matplotlib seaborn numpy
   ```
3. Place the CSV files inside a `/datasets` folder at the root of the project
4. Open the notebook:
   ```bash
   jupyter notebook S7_Version-Estudiante-Project-ConnectaTel.ipynb
   ```
5. Run all cells in order

---

## 🔁 Reproduction Guide

To fully reproduce this analysis:

1. Ensure the three dataset files are available at the paths referenced in the notebook (`/datasets/plans.csv`, `/datasets/users_latam.csv`, `/datasets/usage.csv`)
2. Run cells **sequentially from top to bottom** — each step depends on the output of the previous one
3. No external APIs or credentials are required
4. All visualizations are generated inline using `matplotlib` and `seaborn`
5. The final `user_profile` dataframe (used for segmentation and insights) is built progressively through steps 1–4 — do not skip any cleaning step

---

## 🛠️ Tech Stack

`Python` · `Pandas` · `NumPy` · `Matplotlib` · `Seaborn` · `Jupyter Notebook`

---

## 👤 Author

Data analysis project developed by Camilo Rojas.

Feel free to reach out or connect on [LinkedIn](https://www.linkedin.com/in/camilo-andres-rojas-rojas)
