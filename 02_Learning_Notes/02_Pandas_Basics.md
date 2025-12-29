---
tags:
  - python
  - pandas
  - data
created: {{date}} {{time}}
---

# 🐼 02. Pandas 基礎：處理時間序列數據

## 核心概念：DataFrame
Pandas 的核心是 `DataFrame`，你可以把它想像成一個增強版的 Excel 表格。

對於金融數據，我們通常將 **日期 (Date)** 設為索引 (Index)。

## 1. 建立簡單的 DataFrame

```python
import pandas as pd

data = {
    'Close': [100, 102, 101, 105],
    'Volume': [1000, 1200, 900, 1500]
}
dates = pd.date_range(start='2024-01-01', periods=4)

df = pd.DataFrame(data, index=dates)
print(df)
```

## 2. 選擇數據 (Indexing & Slicing)

- **選擇單列**: `df['Close']`
- **使用標籤選擇 (.loc)**: `df.loc['2024-01-01']`
- **使用位置選擇 (.iloc)**: `df.iloc[0]` (第一行)

## 3. 處理時間序列 (Resampling)
將 1 分鐘 K 線轉為 5 分鐘 K 線。

```python
# 假設 df 是分鐘級數據
# '5T' 代表 5 分鐘 (T = Minute)
# OHLC 聚合邏輯：Open取第一個, High取最大, Low取最小, Close取最後
ohlc_dict = {
    'Open': 'first',
    'High': 'max',
    'Low': 'min',
    'Close': 'last',
    'Volume': 'sum'
}

df_5m = df.resample('5T').agg(ohlc_dict)
```

## 4. 移動視窗 (Rolling Window)
計算移動平均線 (MA) 的基礎。

```python
# 計算 20 日移動平均
df['SMA_20'] = df['Close'].rolling(window=20).mean()
```
