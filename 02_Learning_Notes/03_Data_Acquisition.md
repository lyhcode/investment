---
tags:
  - python
  - data
  - api
created: {{date}} {{time}}
---

# 📥 03. 數據獲取：使用 yfinance

## 簡介
`yfinance` 是一個開源庫，可以免費下載 Yahoo Finance 的歷史數據。

## 1. 下載個股歷史數據

```python
import yfinance as yf

# 下載 Apple (AAPL) 數據
# start: 開始日期, end: 結束日期
df = yf.download('AAPL', start='2020-01-01', end='2024-01-01')

# 查看前 5 行
print(df.head())
```

## 2. 下載多檔股票

```python
tickers = ['AAPL', 'MSFT', 'GOOG']
data = yf.download(tickers, start='2023-01-01')['Close']
print(data.head())
```

## 3. 下載分鐘級數據 (限制由 Yahoo 決定)
通常最近 30-60 天可以下載到分鐘級數據。

```python
# interval: 數據頻率 (1m, 2m, 5m, 15m, 30m, 60m, 90m, 1h, 1d, 5d, 1wk, 1mo, 3mo)
df_idx = yf.download('SPY', period='7d', interval='1m')
```

## 常見問題
- **Adjusted Close (Adj Close)**: 經除權息調整後的收盤價，計算回報率時建議使用此欄位。
- **缺失值**: 遇到國定假日可能會沒有數據，可用 `df.dropna()` 處理。
