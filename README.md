# US SEC Financial Statement (2021-2024)

This repository contains code and a Jupyter notebook to load, process, and compute KPIs from the "US SEC Financial Statement" dataset (quarterly filings 2021-2024).

**Contents**

- `main.ipynb` — Notebook that loads the dataset, filters revenue tags, normalizes columns and computes KPIs.
- `data/sic_mapping.csv` — SIC-to-industry mapping used to derive `industry` values.
- `kdata/Dataset/...` — Quarterly dataset folders (`2022q1`, `2022q2`, ...).
- Dataset details can be found [here](https://kazing98.github.io/us_sec_financial_statement/readme.htm)
- SIC mappings were taken from [here](https://www.sec.gov/search-filings/standard-industrial-classification-sic-code-list) for Industry mapping code.
- SEC Financial Statement Datasets were taken from [here](https://www.sec.gov/data-research/sec-markets-data/financial-statement-data-sets) for Year 2021 till 2024 which were then uploaded to [kaggle](https://www.kaggle.com/datasets/rohanpanda80/us-sec-financial-statement-2021-2024).

**Quick start**

1. Install dependencies (use your environment's preferred method). Example with pip:

```powershell
python -m venv .venv; .\.venv\Scripts\Activate.ps1; pip install -r requirements.txt
```

2. Open and run the notebook `main.ipynb` in Jupyter or VS Code.

3. The notebook will attempt to find the local dataset under `kdata`; if not present it can download from Kaggle (note: Kaggle credentials required).

**Key functions (notebook)**

- `compute_revenue_growth(df)`
  - Adds a `revenue_growth_pct` column to `df` (period-over-period growth in percent) grouped by `cik`.
  - Expects `fiscal_year` (int) and `qtrs` (quarter number 1-4) to be present.
  - Parses quarter numbers from `qtrs` (numeric 1..4) and sorts before computing growth.

- `compute_cagr(df)`
  - Computes the CAGR per `cik` using the first and last available `revenue` observations.
  - Returns the original dataframe merged with a `cagr` column expressed as percent (e.g. `12.34` = 12.34%). Handles missing/zero data safely.

- `compute_quarter_revenue(df)`
  - Aggregates `revenue` by `cik`, `fiscal_year`, and `qtrs` (quarter number 1..4).
  - Returns a tidy DataFrame with `quarter_label` (e.g. `2022Q1`).