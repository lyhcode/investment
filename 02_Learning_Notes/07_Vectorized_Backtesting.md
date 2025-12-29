---
tags:
  - python
  - backtesting
  - pandas
created: {{date}} {{time}}
---

# 🚀 07. 向量化回測實作 (Vectorized Backtesting)

本篇展示如何使用 Pandas 進行快速的向量化回測。

## 範例：雙均線策略回測

```python
import pandas as pd
import yfinance as yf
import matplotlib.pyplot as plt
import numpy as np

# 1. 準備數據
df = yf.download('SPY', start='2015-01-01', end='2023-01-01')
df = df[['Close']].copy()

# 2. 策略邏輯 (Signal Generation)
df['SMA_Fast'] = df['Close'].rolling(window=50).mean()
df['SMA_Slow'] = df['Close'].rolling(window=200).mean()

# 信號: 1 (持有), 0 (空手)
# 注意：這裡使用 shift(1) 是為了避免前視偏差！
# 今天的信號是基於昨天的收盤價計算出來的，所以只能明天執行
df['Signal'] = np.where(df['SMA_Fast'] > df['SMA_Slow'], 1, 0)
df['Position'] = df['Signal'].shift(1) 

# 3. 計算回報 (Return Calculation)
# 市場日報酬率 (log return 通常更適合累加，但簡單起見用 pct_change)
df['Market_Ret'] = df['Close'].pct_change()

# 策略日報酬率 = 持倉狀態 * 市場日報酬率
df['Strategy_Ret'] = df['Position'] * df['Market_Ret']

# 4. 計算累積回報 (Cumulative Return)
# (1 + r).cumprod()
df['Cum_Market'] = (1 + df['Market_Ret']).cumprod()
df['Cum_Strategy'] = (1 + df['Strategy_Ret']).cumprod()

# 5. 結果視覺化
plt.figure(figsize=(10, 6))
df['Cum_Market'].plot(label='Buy & Hold (SPY)')
df['Cum_Strategy'].plot(label='SMA Strategy')
plt.title('Vectorized Backtest: SMA Crossover')
plt.legend()
plt.show()

# 6. 直接比較最終倍數
print(f"Market Return: {df['Cum_Market'].iloc[-1]:.2f}x")
print(f"Strategy Return: {df['Cum_Strategy'].iloc[-1]:.2f}x")
```

## 關鍵點解析
- `shift(1)`: 這是最重要的一步。如果你的訊號是基於今日收盤價 (`Close`) 產生的，你無法在今日收盤前買入（除非你用開盤價回測）。通常假設「今日收盤產生訊號 -> 明日開盤買入」，所以持倉 (`Position`) 要延後一天。
