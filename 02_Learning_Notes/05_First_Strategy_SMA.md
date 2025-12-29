---
tags:
  - python
  - strategy
  - sma
created: {{date}} {{time}}
---

# 📈 05. 第一個策略：雙均線策略 (SMA Crossover)

## 策略邏輯
- **黃金交叉 (Golden Cross)**: 短期均線 (SMA_Short) **向上突破** 長期均線 (SMA_Long) -> **買入訊號**。
- **死亡交叉 (Death Cross)**: 短期均線 **向下跌破** 長期均線 -> **賣出訊號**。

## Python 實作

```python
import pandas as pd
import yfinance as yf
import matplotlib.pyplot as plt

# 1. 獲取數據
df = yf.download('BTC-USD', start='2020-01-01')

# 2. 計算指標
short_window = 50
long_window = 200

df['SMA_50'] = df['Close'].rolling(window=short_window).mean()
df['SMA_200'] = df['Close'].rolling(window=long_window).mean()

# 3. 產生訊號
# 創建一個新的 Signal 欄位，初始化為 0
df['Signal'] = 0.0

# 當 SMA_50 > SMA_200 時標記為 1 (持有/看多)
# 從第 200 天開始切片，因為前面沒有足夠數據計算 SMA_200
df.iloc[long_window:, df.columns.get_loc('Signal')] = \
    np.where(df['SMA_50'][long_window:] > df['SMA_200'][long_window:], 1.0, 0.0)

# 計算買賣點 (Position 用 diff 計算：1-0=1 買入, 0-1=-1 賣出)
df['Position'] = df['Signal'].diff()

# 4. 簡單回測與視覺化
plt.figure(figsize=(14, 7))

# 畫價格
plt.plot(df['Close'], label='Close Price', alpha=0.5)
# 畫均線
plt.plot(df['SMA_50'], label='SMA 50', alpha=0.7)
plt.plot(df['SMA_200'], label='SMA 200', alpha=0.7)

# 標記買入點 (綠色上三角形)
plt.plot(df[df['Position'] == 1].index, 
         df['SMA_50'][df['Position'] == 1], 
         '^', markersize=10, color='g', label='Buy Signal')

# 標記賣出點 (紅色下三角形)
plt.plot(df[df['Position'] == -1].index, 
         df['SMA_50'][df['Position'] == -1], 
         'v', markersize=10, color='r', label='Sell Signal')

plt.title('Bitcoin SMA Crossover Strategy')
plt.legend()
plt.show()
```

## 注意事項
- 這只是最基礎的策略，未考慮手續費與滑點。
- 趨勢策略在盤整市場 (Range-bound market) 表現通常不佳。
