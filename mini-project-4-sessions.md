# Mini‑projekt (Classic mode) — 4 zajęcia: Data → Features → Model

**Tryb:** Classic (syllabus + task lists)  
**Czas:** maks. 4 zajęcia (po ~90 min)  
**Cel:** zbudować mały, kompletny prototyp: automatyczne pobieranie danych → walidacja → EDA + skalowanie → feature engineering → model + walidacja + optymalizacja.  

**English summary:** A 4-session mini project where you automatically fetch data from a provided source URL, validate it, perform EDA and scaling, engineer features, and train + validate + tune a baseline model.

---

## Źródło danych (wymagane, z linku)

Użyj publicznego feedu USGS Earthquakes (GeoJSON), bez klucza API:

- **All earthquakes, last 30 days:** `https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_month.geojson`

To źródło jest:

- proste (JSON),
- stabilne i publiczne,
- aktualizowane automatycznie,
- ma sensowne pola numeryczne + czas + lokalizację (dobry materiał do EDA i feature engineering).

---

## Repo i struktura (wymagane)

Zalecana struktura:

```
mini-project/
├── data_raw/                 # pobrane dane (cache) lub puste + .gitignore
├── data_processed/           # dane po czyszczeniu / transformacjach
├── data_examples/            # małe sample (10–200 wierszy)
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

**Ważne:** pełnych danych nie musisz commitować. W repo ma być:

- skrypt pobierający dane,
- sample w `data_examples/`,
- dokumentacja co i gdzie powstaje.

---

## Definicja problemu modelowania (wybierz jedną opcję)

Wybierz **jedną** i trzymaj się jej w całym projekcie:

### Opcja A — Klasyfikacja (łatwiejsza do interpretacji)

**Cel:** przewidzieć kategorię wielkości wstrząsu:

- label: `is_strong = 1` jeśli `mag >= 5.0`, inaczej `0`.

Metryki:

- F1 (ważne przy niezbalansowanych klasach),
- ROC-AUC (opcjonalnie).

### Opcja B — Regresja

**Cel:** przewidzieć `mag` (magnituda).

Metryki:

- MAE / RMSE.

---

## Zasady jakości (obowiązkowe w całym projekcie)

- Każdy krok ma mieć **runnable script** w `scripts/`.
- Każdy krok ma mieć krótką notatkę w `documentation/`.
- Zapisuj kluczowe artefakty:
  - sample danych,
  - wykresy EDA,
  - tabelkę feature’ów,
  - wyniki metryk i porównanie modeli.
- Ustaw **random seed** i opisz go w dokumentacji.

---

# Zajęcia 1/4 — Pobranie danych + walidacja (automatycznie)

## Wymagania (deliverables)

1. Skrypt `scripts/01_fetch_and_validate.py`, który:
   - pobiera dane z linku,
   - zapisuje surowy plik (np. `data_raw/usgs_all_month.geojson`),
   - wyciąga do tabeli (np. CSV/Parquet) minimalny zestaw pól:
     - `time`, `mag`, `place`, `latitude`, `longitude`, `depth`
     - (opcjonalnie) `id`, `type`, `status`, `tsunami`, `sig`
2. Prosta walidacja (minimum):
   - sprawdź, czy dataset nie jest pusty,
   - sprawdź, czy `mag` jest liczbą i nie jest absurdalna (np. `mag < -1` lub `mag > 10`),
   - sprawdź brakujące wartości w kluczowych kolumnach (`mag`, `time`, `lat`, `lon`).
3. Raport `documentation/01_data_ingestion_validation.md`:
   - skąd dane (link),
   - ile rekordów pobrano,
   - jakie walidacje wykonałeś i jaki był wynik,
   - gdzie zapisujesz outputy (ścieżki).
4. Sample do repo:
   - `data_examples/raw_sample.json` lub `raw_sample.csv` (np. 20 rekordów).

## Minimalny próg zaliczenia

- pobranie działa “od zera”,
- walidacja raportuje wyniki i nie jest tylko opisem.

---

# Zajęcia 2/4 — EDA + normalizacja/standaryzacja

## Wymagania (deliverables)

1. Skrypt `scripts/02_eda_and_scaling.py`, który:
   - ładuje dane po kroku 1,
   - wykonuje EDA:
     - rozkład `mag` (histogram),
     - brakujące wartości (tabela lub heatmapa),
     - korelacje podstawowych cech numerycznych (np. `mag`, `depth`, `sig` jeśli używasz),
   - przygotowuje wersję danych do modelowania:
     - imputacja braków (opisana),
     - **standaryzacja** (np. StandardScaler) lub **normalizacja** (MinMaxScaler) — uzasadnij wybór.
2. Raport `documentation/02_eda_scaling.md`:
   - 3–5 najważniejszych obserwacji z EDA,
   - co zrobiłeś z brakami,
   - co i jak skalowałeś, dlaczego.
3. Artefakty w repo:
   - 2–4 wykresy w `data_examples/eda/` (PNG),
   - sample danych po przetworzeniu (`data_examples/processed_sample.csv`).

---

# Zajęcia 3/4 — Feature engineering

## Wymagania (deliverables)

1. Skrypt `scripts/03_feature_engineering.py`, który tworzy min. **8 cech** (z czego min. 4 “nieliniowe” / pochodne), np.:
   - czas:
     - `hour_of_day`, `day_of_week`, `month`
   - geografia:
     - “grid cell” (np. zaokrąglenie lat/lon do 0.5°) jako kategoria,
     - odległość od ustalonego punktu referencyjnego (np. środek Europy) — cecha numeryczna
   - sygnał:
     - `mag_depth_interaction = mag * depth`
     - `log_depth = log(1+depth)`
   - tekst:
     - proste cechy z `place` (np. długość stringa, czy zawiera nazwę regionu), **bez** ciężkich NLP
2. Raport `documentation/03_feature_engineering.md`:
   - tabelka “feature → jak liczony → po co”,
   - informacja, które cechy są numeryczne/kategoryczne,
   - gdzie zapisujesz finalną macierz cech (format + ścieżka + shape).
3. Artefakty:
   - `data_examples/feature_matrix_sample.csv` (np. 50 wierszy),
   - krótka tabelka (w MD) z `shape` i listą kolumn.

---

# Zajęcia 4/4 — Model + walidacja + optymalizacja

## Wymagania (deliverables)

1. Skrypt `scripts/04_train_and_validate.py`, który:
   - ładuje feature matrix,
   - robi split train/test (lub train/valid/test) z seedem,
   - trenuje co najmniej **2 modele**:
     - baseline (np. Logistic Regression / Linear Regression),
     - model mocniejszy (np. RandomForest / GradientBoosting / XGBoost jeśli umiesz).
   - waliduje metrykami właściwymi dla problemu (A: F1/AUC, B: MAE/RMSE),
   - zapisuje wyniki do pliku (np. `data_examples/model_results.json`).
2. Optymalizacja (minimum):
   - prosty tuning 1–2 hiperparametrów (GridSearchCV lub ręczny sweep),
   - porównanie wyników “przed/po”.
3. Raport `documentation/04_modeling.md`:
   - definicja targetu,
   - metryki i wyniki (tabela),
   - interpretacja: co jest największym ograniczeniem (dane? cechy? metryka?),
   - co byś zrobił dalej.

---

## Kryteria oceny (proste)

- **Reprodukowalność (30%)**: skrypty działają od zera; ścieżki i zależności opisane.
- **Jakość danych i walidacja (20%)**: walidacja + sensowna obsługa braków.
- **EDA + skalowanie (15%)**: poprawne i uzasadnione.
- **Feature engineering (15%)**: sensowne cechy, opisane “po co”.
- **Model + walidacja + tuning (20%)**: co najmniej 2 modele + metryki + minimalny tuning.

---

## Minimalne wymagania techniczne

- Python 3.10+
- Biblioteki (wystarczy podzbiór): `pandas`, `numpy`, `requests`, `scikit-learn`, `matplotlib` (opcjonalnie `pyarrow` dla Parquet).

---

## “How to run” (wymagane w `README.md`)

W `README.md` umieść:

1. instalację zależności,
2. kolejność uruchomienia skryptów 01 → 04,
3. gdzie powstają outputy i sample.

