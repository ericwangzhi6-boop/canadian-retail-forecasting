# Canadian Retail Sales Forecasting & Macro Driver Analysis

## Overview
This self-directed Python analytics project explores whether historical retail sales patterns and selected macroeconomic indicators can improve one-month-ahead Canadian retail sales forecasts. It was used to refresh and apply Python, data analysis, statistics, and introductory machine-learning concepts in a business context.

## Data Sources
Only public national aggregate data are used.

| Source | Series / selection | Local file |
|---|---|---|
| [Statistics Canada 20-10-0067-01](https://www150.statcan.gc.ca/t1/tbl1/en/tv.action?pid=2010006701) | Monthly retail sales, seasonally adjusted; Canada, Retail trade [44-45], current prices, CAD millions | `data/raw/retail_sales_raw.csv` |
| [Statistics Canada 18-10-0006-01](https://www150.statcan.gc.ca/t1/tbl1/en/tv.action?pid=1810000601) | Seasonally adjusted CPI; Canada, All-items, 2002=100; vector v41690914 | `data/raw/cpi_raw.csv` |
| [Statistics Canada 14-10-0287-01](https://www150.statcan.gc.ca/t1/tbl1/en/tv.action?pid=1410028701) | Unemployment rate; Canada, Total - Gender, 15 years and over, Estimate, Seasonally adjusted | `data/raw/unemployment_raw.csv` |
| [Bank of Canada Valet API](https://www.bankofcanada.ca/valet/docs) | Policy rate, V39079; last observation each month | `data/raw/V39079.json` |
| [Bank of Canada Valet API](https://www.bankofcanada.ca/valet/docs) | USD/CAD, FXUSDCAD; monthly mean | `data/raw/FXUSDCAD.json` |

The notebook downloads missing files directly from the official sources. Statistics Canada CSVs are local subsets of the full public tables, retaining Canada through June 2026. Start dates are January 2017 for retail, January 1992 for CPI, and January 1976 for labour data. The original experiment ends June 2026; the chronological split uses feature months before January 2025 for training and January 2025 onward for testing. Targets are the following month's sales.

To download manually, use the English full-table CSV ZIP links: [retail](https://www150.statcan.gc.ca/n1/tbl/csv/20100067-eng.zip), [CPI](https://www150.statcan.gc.ca/n1/tbl/csv/18100006-eng.zip), [labour](https://www150.statcan.gc.ca/n1/tbl/csv/14100287-eng.zip). Prefer the notebook's automatic downloader, which applies the date and country selections consistently. Do not substitute the unadjusted CPI table 18-10-0004.

Downloaded data and derived CSVs are excluded from version control. Existing caches are reused for repeat runs. Removing cached files downloads current public releases; historical revisions can change results. `data/raw/sources.json` records the locally prepared Statistics Canada snapshots and checksums. This is a current-vintage retrospective exercise, not a reconstruction of information available at each historical forecast date.

## Tools
Python, pandas, NumPy, matplotlib, scikit-learn, requests, Jupyter, and the Bank of Canada Valet API.

## Methods
- Data cleaning, country/series filtering, and monthly merging.
- Lag and rolling features; month-over-month and year-over-year growth analysis.
- Chronological train/test split and a naive next-month baseline.
- Linear Regression, standardized Ridge Regression, and Random Forest Regression.
- A growth-target Random Forest experiment, converted back to sales levels.
- Evaluation using MAE, RMSE, and R².

## Key Findings
In the original experiment, the naive baseline was a strong benchmark (MAE approximately CAD 620M), and ordinary Linear Regression did not outperform it. Ridge using historical sales features performed best among the tested specifications (MAE approximately CAD 537M, about a 13% reduction).

Direct sales-level Random Forest performed poorly, consistent with its difficulty extrapolating beyond the training target range. Predicting next-month growth substantially improved Random Forest performance, but Ridge remained better. Adding the selected contemporaneous macro variables did not improve forecast accuracy in the tested specifications. These findings apply to this dataset and experiment, not to forecasting generally.

The clean local run on 22 September 2026 reproduced these results using newly downloaded official public data:

| Model | MAE (CAD millions) | RMSE (CAD millions) | R² |
|---|---:|---:|---:|
| Ridge — History | 537.10 | 638.16 | 0.802 |
| Random Forest — Growth Target | 589.36 | 666.65 | 0.784 |
| Random Forest — Growth + Macro | 618.11 | 723.25 | 0.745 |
| Naive Baseline | 619.94 | 690.85 | 0.768 |
| Linear Regression — History | 737.11 | 871.97 | 0.630 |
| Random Forest — Level | 1384.25 | 1873.99 | -0.710 |

Validation: all 122 cells completed in a fresh project kernel (108 code cells), with no cell errors or stderr outputs. Package dependency checks passed. Saved outputs were inspected again for private paths, identifiers, credentials, attachments, and unexpected URLs. Raw data are public source downloads; no previous-machine datasets were used.

## Run Locally
The project was validated with Python 3.8.3 and the package versions in `requirements.txt`. These pins reproduce the available local environment; they are not a claim that these are the latest releases.

From the project root on macOS:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
python -m ipykernel install --sys-prefix --name python3 --display-name "Python 3 (project .venv)"
python -m jupyter lab notebooks/01_retail_sales.ipynb
```

Select the project `.venv` kernel, restart it, and run all cells. Launch from the project root or `notebooks/`. Internet access is required on the first run; cached public files permit subsequent runs without new downloads. The notebook writes `data/processed/retail_model_dataset.csv` and regenerates its charts and result tables. The processed file includes lag-related missing values and the unavailable final next-month target; modeling cells explicitly drop incomplete rows.

## Limitations
- Relatively small monthly sample and a major COVID structural shock.
- Macroeconomic and retail publication lags are not fully modeled; revised historical data can overstate real-time usefulness.
- Limited models and hyperparameter tuning; repeated comparison on one holdout does not constitute an independent final evaluation.
- Findings depend on the selected sample, features, and data vintage.
- A self-directed learning exercise, not a production forecasting system.

## Project Files
```text
data/raw/          # local public data caches; ignored
data/processed/    # regenerated modeling dataset; ignored
notebooks/01_retail_sales.ipynb
README.md
requirements.txt
.gitignore
```

The virtual environment, local backups, credentials, editor settings, and temporary files are ignored. Review notebook outputs before any future public upload: ignore rules do not sanitize content embedded inside a notebook. No publication step is part of running this project.
