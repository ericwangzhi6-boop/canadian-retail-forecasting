# Canadian Retail Sales Forecasting

This is a small personal project I built to refresh my Python and data analysis skills.

I chose Canadian retail sales because the topic is closely related to my background in FP&A and Treasury. The main question I wanted to explore was:

**Can recent retail sales patterns help forecast next month's Canadian retail sales, and do macroeconomic variables add useful information?**

## Data

The project uses publicly available data from Statistics Canada and the Bank of Canada, including:

- Canadian retail sales
- CPI / inflation
- unemployment rate
- Bank of Canada policy rate
- USD/CAD exchange rate

## What I did

I used Python to clean and combine the datasets, create lag and rolling features, and compare several simple forecasting approaches.

The models I tested included:

- a naive baseline
- Linear Regression
- Ridge Regression
- Random Forest

I also tested whether predicting next-month growth instead of the absolute sales level would improve the Random Forest model.

## Results

The naive baseline was already quite strong.

Among the models I tested, the history-only Ridge Regression model performed best, with an MAE of about CAD 537 million compared with about CAD 620 million for the baseline, a 13.4% reduction in the selected test period.

The Random Forest model performed poorly when predicting the sales level directly, but improved when I predicted next-month sales growth with a smaller feature set. Since both changed, I cannot attribute all of the improvement to the target alone.

Adding the selected macroeconomic variables did not improve short-term forecast accuracy in this project.

## What I learned

The main takeaway for me was that a more complicated model does not automatically produce a better forecast.

This project also gave me a chance to practice:

- working with public datasets and APIs
- pandas and NumPy
- data cleaning and feature engineering
- time-based train/test splits
- basic regression and machine-learning models
- evaluating models with MAE, RMSE, and R²

## Notes

This is a self-directed learning project rather than a production forecasting system. The dataset is relatively small and includes the COVID period, and I did not attempt extensive model tuning. The analysis uses national totals and the same short test period for model comparison. Retail and macroeconomic publication lags were not fully modeled, and the data include historical revisions. This is not a real-time backtest.
## Run the notebook

The tested environment is Python 3.8.3 with the versions in `requirements.txt`. From the project folder on macOS:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
python -m ipykernel install --sys-prefix --name python3 --display-name "Python 3 (project .venv)"
python -m jupyter lab notebooks/01_retail_sales.ipynb
```

Select the project `.venv` kernel, restart it, and run all cells. The notebook downloads missing public files and reuses local copies in `data/raw/`. It writes `data/processed/retail_model_dataset.csv`. Internet access is needed for missing files; refreshing the data later may change results because official series are revised.

The source tables are [retail 20-10-0067-01](https://www150.statcan.gc.ca/t1/tbl1/en/tv.action?pid=2010006701), [seasonally adjusted CPI 18-10-0006-01](https://www150.statcan.gc.ca/t1/tbl1/en/tv.action?pid=1810000601), and [labour 14-10-0287-01](https://www150.statcan.gc.ca/t1/tbl1/en/tv.action?pid=1410028701), plus Bank of Canada Valet series V39079 and FXUSDCAD. The notebook keeps the original analysis end date of June 2026. Its 17 test targets cover February 2025 to June 2026.

Data, the virtual environment, local backups, and temporary files are ignored by Git. The notebook and README are not ignored.
