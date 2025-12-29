---
tags:
  - python
  - metrics
  - backtesting
created: {{date}} {{time}}
---

# 📊 08. 回測績效指標 (Performance Metrics)

看策略好壞不能只看「賺多少」，還要看「風險多少」。

## 1. 夏普比率 (Sharpe Ratio)
衡量每承擔一單位風險，能獲得多少超額報酬。
$$Sharpe = \frac{R_p - R_f}{\sigma_p}$$
- $R_p$: 策略年化報酬率
- $R_f$: 無風險利率 (可設為 0 或美債利率)
- $\sigma_p$: 策略年化波動率

```python
import numpy as np

# 假設 returns 是日報酬率 Series
def calculate_sharpe(returns, risk_free_rate=0.0):
    excess_returns = returns - risk_free_rate/252
    return np.sqrt(252) * excess_returns.mean() / excess_returns.std()
```
*註：`np.sqrt(252)` 是將日波動轉為年化波動。*

## 2. 最大回撤 (Maximum Drawdown, MDD)
資產從最高點回落的最大幅度。衡量策略最慘會賠多少。

```python
def calculate_mdd(cum_returns):
    # cum_returns 是累積淨值 (e.g., 1.0, 1.02, 0.98...)
    peak = cum_returns.cummax()
    drawdown = (cum_returns - peak) / peak
    return drawdown.min()
```

## 3. 年化複合成長率 (CAGR)
幾何平均年報酬率。

```python
def calculate_cagr(end_value, start_value, years):
    return (end_value / start_value) ** (1 / years) - 1
```

## 4. 勝率 (Win Rate)
賺錢交易次數佔總交易次數的比例。
**注意**: 高勝率不代表高獲利 (e.g., 賺 1 元 9 次，賠 10 元 1 次)。

```python
# 假設 trade_rets 是每筆交易的報酬率列表
win_rate = len(trade_rets[trade_rets > 0]) / len(trade_rets)
```

## 綜合評估程式碼範例

```python
strategy_rets = df['Strategy_Ret'] # 從上一章取得

sharpe = calculate_sharpe(strategy_rets)
mdd = calculate_mdd(df['Cum_Strategy'])
total_ret = df['Cum_Strategy'].iloc[-1] - 1

print(f"Sharpe Ratio: {sharpe:.2f}")
print(f"Max Drawdown: {mdd:.2%}")
print(f"Total Return: {total_ret:.2%}")
```
