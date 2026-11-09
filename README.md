# Gold Price Prediction (Time Series)

**Type:** Time series forecasting

## Goal
Predict the **daily gold price** (closing price of Gold Futures, USD per troy ounce).

## Data
- **Source page:** [Yahoo Finance - Gold Futures (GC=F)](https://finance.yahoo.com/quote/GC=F/history/)
- **How it was downloaded:** with the `yfinance` Python library (see the code below)
- **Saved file:** `data/gold_price_daily.csv` (5,463 trading days, 2005-01-03 to 2026-09-25)
- **Columns:** Date, Open, High, Low, Close, Volume
- **Alternative source (monthly prices since 1833):** https://github.com/datasets/gold-prices

To download fresh data:
```python
import yfinance as yf
df = yf.download("GC=F", start="2005-01-01", auto_adjust=False)
df.columns = df.columns.get_level_values(0)
df[["Open", "High", "Low", "Close", "Volume"]].reset_index().to_csv("data/gold_price_daily.csv", index=False)
```

## Files
| File | Description |
|---|---|
| `gold_price_prediction.ipynb` | The full project notebook |
| `data/gold_price_daily.csv` | The dataset |
| `data/gold_price_daily_metadata.xlsx` | Metadata: source, date range, description and statistics of every column |
| `models/` | Created when you run the notebook (best model + feature list) |

## Steps in the notebook
1. Load the data
2. Data cleaning - sort by date, duplicate dates, missing values, business-day frequency (fill holidays), outlier check
3. EDA - price chart, moving averages, yearly average, daily returns, seasonal decomposition, ADF stationarity test, ACF / PACF
4. Feature creation - lag features, rolling mean / std, day of week, month
5. **Time based split** - 80% train (oldest) / 15% test / 5% unseen (newest), no shuffling
6. Modeling - Naive, Moving Average, Holt Exponential Smoothing, ARIMA (multi-step and one-step), Linear Regression, Ridge, Lasso, Random Forest, Gradient Boosting, XGBoost (+ TimeSeriesSplit cross validation and grid search)
7. Training and testing evaluation - MAE, RMSE, MAPE, R2
8. Compare the models, save the best model, load it and predict the unseen 5%
9. Forecast the next 30 business days with ARIMA

## How to run
```bash
pip install pandas numpy matplotlib seaborn statsmodels scikit-learn xgboost joblib jupyter
```
Open `gold_price_prediction.ipynb` **from this folder** and run all cells.
