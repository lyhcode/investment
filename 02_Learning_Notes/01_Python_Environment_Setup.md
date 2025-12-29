---
tags:
  - python
  - setup
  - investment
created: {{date}} {{time}}
---

# 🐍 01. Python 環境建置與基礎套件

## 為什麼選擇 Python？
Python 擁有豐富的數據分析與金融庫（Libraries），是自動化交易的首選語言。

## 1. 安裝 Python (推薦 Anaconda/Miniconda)
建議使用 **Miniconda** 以保持環境輕量。
- 下載地址: [Miniconda](https://docs.conda.io/en/latest/miniconda.html)

## 2. 建立虛擬環境 (Virtual Environment)
避免不同專案間的套件衝突。

```bash
# 建立名為 trading 的環境，指定 Python 3.10
conda create -n trading python=3.10

# 啟用環境
conda activate trading
```

## 3. 安裝核心套件
自動化交易的 "三劍客" + 數據源：

```bash
pip install pandas numpy matplotlib yfinance mplfinance jupyterlab
```

- **pandas**: 處理表格數據 (DataFrame)，金融數據分析的核心。
- **numpy**: 數值計算。
- **matplotlib / mplfinance**: 繪圖與視覺化。
- **yfinance**: 下載 Yahoo Finance 歷史股價數據。
- **jupyterlab**: 交互式開發環境，適合研究策略。

## 4. 測試安裝
打開終端機輸入 `python`，然後嘗試匯入：

```python
import pandas as pd
import yfinance as yf
print("Setup Complete!")
```
