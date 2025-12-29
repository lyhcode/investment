---
tags:
  - python
  - visualization
  - matplotlib
created: {{date}} {{time}}
---

# 📊 04. 數據視覺化：Matplotlib 與 mplfinance

## 1. Matplotlib 基礎繪圖
最基礎的折線圖，適合畫收盤價。

```python
import matplotlib.pyplot as plt
import yfinance as yf

df = yf.download('TSLA', start='2023-01-01')

plt.figure(figsize=(12, 6))
plt.plot(df.index, df['Close'], label='Tesla Close Price')
plt.title('Tesla Stock Price')
plt.xlabel('Date')
plt.ylabel('Price')
plt.legend()
plt.grid(True)
plt.show() # 在 Jupyter 中顯示
```

## 2. 繪製專業 K 線圖 (Candlestick Chart)
使用 `mplfinance` 套件，這是最簡單的方法。

```python
import mplfinance as mpf

# 畫最近 50 天的 K 線，並疊加 10日與20日均線
# type='candle': 蠟燭圖
# mav=(10, 20): 移動平均線
# volume=True: 顯示成交量
mpf.plot(df[-50:], type='candle', mav=(10, 20), volume=True, style='charles')
```

## 3. 視覺化的重要性
- **確認數據品質**: 有無異常的高價或低價？
- **觀察趨勢**: 肉眼識別支撐與壓力位。
