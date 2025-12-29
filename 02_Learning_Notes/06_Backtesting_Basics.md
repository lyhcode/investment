---
tags:
  - backtesting
  - theory
  - investment
created: {{date}} {{time}}
---

# 🧪 06. 回測基礎導論 (Backtesting Basics)

## 什麼是回測？
回測是指使用**歷史數據**來模擬策略的表現，以評估其在未來的潛在獲利能力。

> **核心假設**：歷史會在某種程度上重演 (History tends to repeat itself)。

## 回測的兩種類型

### 1. 向量化回測 (Vectorized Backtesting)
- **原理**: 利用 Pandas 等陣列運算，一次性計算所有時間點的信號與損益。
- **優點**: 速度極快，程式碼簡潔，適合初步篩選策略。
- **缺點**: 很難模擬複雜的交易機制（如滑點、部分成交、動態止損）。
- **適用**: 研究階段 (Research Phase)。

### 2. 事件驅動回測 (Event-Driven Backtesting)
- **原理**: 模擬真實市場的時間流，逐筆 (Tick-by-tick) 或逐根 K 線 (Bar-by-bar) 處理事件 (Market Event -> Signal Event -> Order Event -> Fill Event)。
- **優點**: 極度擬真，可考慮流動性、滑點、延遲。
- **缺點**: 開發複雜，運算速度較慢。
- **適用**: 實盤前驗證 (Pre-deployment Validation)。

## 常見陷阱 (Pitfalls)

### 1. 前視偏差 (Look-ahead Bias)
**定義**: 在計算 $t$ 時刻的信號時，用到了 $t+1$ 時刻或未來的數據。
**例子**: 使用當天的「收盤價」來決定當天的「開盤買入」。(不可能做到)
**解法**: 永遠只用 $t-1$ 的收盤數據來決定 $t$ 時刻的動作，或者確保交易發生在信號產生之後。

### 2. 過度擬合 (Overfitting)
**定義**: 策略對歷史數據優化過度，導致在樣本外 (Out-of-Sample) 表現極差。常被稱為 "Curve Fitting"。
**徵兆**: 參數稍微調整（例如 MA 從 20 改為 21），績效就崩盤。
**解法**: 減少參數數量、使用訓練集/測試集 (In-Sample/Out-of-Sample) 驗證。
