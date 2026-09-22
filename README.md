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

Among the models I tested, the history-only Ridge Regression model performed best, with an MAE of about CAD 537 million compared with about CAD 620 million for the baseline.

The Random Forest model performed poorly when predicting the sales level directly, but improved significantly when I changed the target to next-month sales growth.

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

This is a self-directed learning project rather than a production forecasting system. The dataset is relatively small and includes the COVID period, and I did not attempt extensive model tuning.