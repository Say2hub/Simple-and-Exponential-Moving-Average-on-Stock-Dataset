# Simple-and-Exponential-Moving-Average-on-Stock-Dataset
This repository contains a Jupyter Notebook for a financial data analysis assignment focused on **Simple Moving Average (SMA)** and **Exponential Moving Average (EMA)** concepts using historical stock data.

The notebook performs:
- Data loading and exploration
- Basic data cleaning/preparation
- Monthly resampling of daily stock data
- Generation of separate monthly aggregated CSV files for each stock ticker

## Dataset Overview (`output_file.csv`)

- **Rows**: 5,030
- **Columns**: date, volume, open, high, low, close, adjclose, ticker
- **Tickers** (10 unique): AAPL, AVGO, CSCO, PEP, TMUS, TSLA, and others
- **Date Range**: Approximately 2018 to 2019 (daily data)

## 📊 Results Analysis

After running the notebook, monthly aggregated stock data is generated for each ticker and saved as individual CSV files (`result_{ticker}.csv`).

### Key Observations from the Generated Results

1. **Monthly Aggregation**:
   - Daily data is resampled to **monthly frequency** using the mean for numerical columns (open, high, low, close, adjclose, volume).
   - This reduces noise and provides a clearer view of longer-term trends.

2. **Output Files**:
   - One CSV file per ticker (e.g., `result_AAPL.csv`, `result_TSLA.csv`, etc.).
   - Each file contains monthly averages with the original date index preserved (typically the first or last trading day of the month, depending on resampling alignment).

3. **Compute SMA and EMA Values for Each Ticker**
   ### Efficient Moving Average Calculation (GroupBy + Transform)

To compute **Simple Moving Average (SMA)** and **Exponential Moving Average (EMA)** for each ticker **without looping**, use `groupby('ticker').transform()` with rolling/ewm operations. This keeps the original DataFrame structure and applies the calculation independently per ticker.

```python
# Simple Moving Averages
monthly['SMA_10'] = monthly.groupby('ticker')['close'].transform(
    lambda x: x.rolling(window=10, min_periods=1).mean()
)

monthly['SMA_20'] = monthly.groupby('ticker')['close'].transform(
    lambda x: x.rolling(window=20, min_periods=1).mean()
)

# Exponential Moving Averages
monthly['EMA_10'] = monthly.groupby('ticker')['close'].transform(
    lambda x: x.ewm(span=10, adjust=False).mean()
)

monthly['EMA_20'] = monthly.groupby('ticker')['close'].transform(
    lambda x: x.ewm(span=20, adjust=False).mean()
)
```



