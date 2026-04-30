# Mini Project (Classic mode) — 4 sessions: Data → Features → Model

**Mode:** Classic (syllabus + task lists)  
**Time:** maximum 4 sessions (~90 minutes each)  
**Goal:** build a small end-to-end prototype: automatic data fetching → validation → EDA + scaling → feature engineering → model training + validation + optimisation.  

---

## Data source (required, from a URL)

Use the public USGS Earthquakes feed (GeoJSON), no API key required:

- **All earthquakes, last 30 days:** `https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_month.geojson`

This source is:

- simple (JSON),
- stable and public,
- automatically updated,
- contains meaningful numeric fields + time + location (good for EDA and feature engineering).

---

## Repository and structure (required)

Recommended structure:

```
mini-project/
├── data_raw/                 # downloaded data (cache) OR empty + .gitignore
├── data_processed/           # cleaned / transformed data
├── data_examples/            # small samples (10–200 rows)
├── scripts/
│   ├── 01_fetch_and_validate.py
│   ├── 02_eda_and_scaling.py
│   ├── 03_feature_engineering.py
│   └── 04_train_and_validate.py
├── documentation/
│   ├── 01_data_ingestion_validation.md
│   ├── 02_eda_scaling.md
│   ├── 03_feature_engineering.md
│   └── 04_modeling.md
├── requirements.txt
└── README.md
```

**Important:** you do not need to commit the full dataset. Your repository must include:

- a script that fetches the data,
- samples in `data_examples/`,
- documentation of what is produced and where.

---

## Modelling problem definition (pick one option)

Pick **one** and use it consistently throughout the project:

### Option A — Classification (easier to interpret)

**Goal:** predict whether an earthquake is “strong”:

- label: `is_strong = 1` if `mag >= 5.0`, otherwise `0`.

Metrics:

- F1 (important for imbalanced classes),
- ROC-AUC (optional).

### Option B — Regression

**Goal:** predict `mag` (magnitude).

Metrics:

- MAE / RMSE.

---

## Quality rules (mandatory for the whole project)

- Each step must have a **runnable script** in `scripts/`.
- Each step must have a short note in `documentation/`.
- Save key artifacts:
  - data samples,
  - EDA plots,
  - a feature table,
  - metrics results and a model comparison.
- Set a **random seed** and document it.

---

# Session 1/4 — Data fetching + validation (automatic)

## Requirements (deliverables)

1. Script `scripts/01_fetch_and_validate.py` that:
   - downloads data from the URL,
   - saves the raw file (e.g., `data_raw/usgs_all_month.geojson`),
   - extracts a minimal set of fields into a table (CSV/Parquet), for example:
     - `time`, `mag`, `place`, `latitude`, `longitude`, `depth`
     - (optional) `id`, `type`, `status`, `tsunami`, `sig`
2. Simple validation (minimum):
   - check the dataset is not empty,
   - check `mag` is numeric and not absurd (e.g., `mag < -1` or `mag > 10`),
   - check missing values in key columns (`mag`, `time`, `lat`, `lon`).
3. Report `documentation/01_data_ingestion_validation.md`:
   - data source (URL),
   - how many records were downloaded,
   - which validations you executed and the result,
   - where outputs are written (paths).
4. Repo samples:
   - `data_examples/raw_sample.json` or `raw_sample.csv` (e.g., 20 records).

## Minimum pass threshold

- fetching works “from scratch”,
- validation reports results (not only a description).

---

# Session 2/4 — EDA + normalisation/standardisation

## Requirements (deliverables)

1. Script `scripts/02_eda_and_scaling.py` that:
   - loads the dataset from Session 1,
   - performs EDA:
     - distribution of `mag` (histogram),
     - missing values (table or heatmap),
     - correlations of basic numeric features (e.g., `mag`, `depth`, `sig` if used),
   - prepares a modelling-ready dataset:
     - missing value imputation (documented),
     - **standardisation** (e.g., StandardScaler) or **normalisation** (MinMaxScaler) — justify your choice.
2. Report `documentation/02_eda_scaling.md`:
   - 3–5 key observations from EDA,
   - what you did with missing values,
   - what and how you scaled, and why.
3. Repo artifacts:
   - 2–4 plots in `data_examples/eda/` (PNG),
   - a processed sample (`data_examples/processed_sample.csv`).

---

# Session 3/4 — Feature engineering

## Requirements (deliverables)

1. Script `scripts/03_feature_engineering.py` that creates at least **8 features** (at least 4 “non-linear” / derived), for example:
   - time-based:
     - `hour_of_day`, `day_of_week`, `month`
   - geography:
     - “grid cell” (e.g., rounding lat/lon to 0.5°) as a categorical feature,
     - distance to a fixed reference point (e.g., the geographic centre of Europe) — numeric feature
   - signal/transformations:
     - `mag_depth_interaction = mag * depth`
     - `log_depth = log(1+depth)`
   - text (lightweight, no heavy NLP):
     - simple features from `place` (string length, contains region keywords, etc.)
2. Report `documentation/03_feature_engineering.md`:
   - a table “feature → how computed → why”,
   - which features are numeric vs categorical,
   - where you save the final feature matrix (format + path + shape).
3. Artifacts:
   - `data_examples/feature_matrix_sample.csv` (e.g., 50 rows),
   - a short table (in the MD) with `shape` and a column list.

---

# Session 4/4 — Model training + validation + optimisation

## Requirements (deliverables)

1. Script `scripts/04_train_and_validate.py` that:
   - loads the feature matrix,
   - performs a train/test split (or train/valid/test) with a fixed seed,
   - trains at least **two models**:
     - a baseline (e.g., Logistic Regression / Linear Regression),
     - a stronger model (e.g., RandomForest / GradientBoosting / XGBoost if you know it).
   - validates using problem-appropriate metrics (A: F1/AUC, B: MAE/RMSE),
   - saves results to a file (e.g., `data_examples/model_results.json`).
2. Optimisation (minimum):
   - simple tuning of 1–2 hyperparameters (GridSearchCV or a manual sweep),
   - compare results “before vs after”.
3. Report `documentation/04_modeling.md`:
   - target definition,
   - metrics and results (table),
   - interpretation: what is the biggest limitation (data? features? metric?),
   - what you would do next.

---

## Grading criteria (simple)

- **Reproducibility (30%)**: scripts run from scratch; paths and dependencies documented.
- **Data quality and validation (20%)**: validation + sensible missing-value handling.
- **EDA + scaling (15%)**: correct and justified.
- **Feature engineering (15%)**: meaningful features with clear “why”.
- **Model + validation + tuning (20%)**: at least 2 models + metrics + minimal tuning.

---

## Minimum technical requirements

- Python 3.10+
- Libraries (a subset is enough): `pandas`, `numpy`, `requests`, `scikit-learn`, `matplotlib` (optionally `pyarrow` for Parquet).

---

## “How to run” (required in `README.md`)

In `README.md`, include:

1. dependency installation,
2. the run order for scripts 01 → 04,
3. where outputs and samples are written.

