# 🔬 Crypto Trading Strategy Research

> From classic technical indicators to machine learning — validating trading hypotheses with data, and letting results override intuition.

[🇨🇳 中文版](./README_CN.md)

## 📌 Overview

This is not a project about "finding a profitable strategy". It's a systematic, data-driven research into **whether common trading strategies actually work**.

- Subject: BTC/USDT, 2021-2024, 35,000+ hourly candles
- Key finding: **Complexity does not equal profitability — simple rules beat ML, and buy-and-hold beat all active strategies.**

## 🧠 Research Flow

```mermaid
graph TD
    A["🗂 NB1: Data Pipeline"]:::data
    A -->|"35,064 candles\n99.96% complete"| B["📈 NB2: Classic Indicators"]:::strat
    B -->|"All negative returns\nStop-loss made it worse"| C["📊 NB3: Statistical Strategy"]:::strat
    C -->|"Z-score: first positive return\n+35% but still < buy & hold"| D["🤖 NB4: ML Signals"]:::strat
    D -->|"Overfitting\nWorse than simple rules"| E["⚙️ NB5: Backtest Engine"]:::engine
    E -->|"Added slippage\nReturns halved"| F["📉 NB6: Performance Analysis"]:::analysis
    F -->|"Multi-dimensional\ncomparison"| G["💡 NB7: Conclusions"]:::insight

    classDef data fill:#d1ecf1,stroke:#0c7ea8,color:#0c5a77,stroke-width:2px
    classDef strat fill:#e8daef,stroke:#7d3c98,color:#4a235a,stroke-width:2px
    classDef engine fill:#fadbd8,stroke:#c0392b,color:#78281f,stroke-width:2px
    classDef analysis fill:#fef9e7,stroke:#d4ac0d,color:#7d6608,stroke-width:2px
    classDef insight fill:#d5f5e3,stroke:#1e8449,color:#145a32,stroke-width:2px
```

## 🔑 Key Findings

### 1. Complexity ≠ Effectiveness

```mermaid
graph LR
    A["🟢 Low complexity\nBuy & Hold\nSharpe 0.54"]:::good --> B["🟡 Medium complexity\nZ-score Mean Reversion\nSharpe 0.10~0.19"]:::mid
    B --> C["🔴 High complexity\nXGBoost ML\nSharpe -1.29"]:::bad

    classDef good fill:#d5f5e3,stroke:#1e8449,color:#145a32,stroke-width:2px
    classDef mid fill:#fef9e7,stroke:#d4ac0d,color:#7d6608,stroke-width:2px
    classDef bad fill:#fadbd8,stroke:#c0392b,color:#78281f,stroke-width:2px
```

### 2. Performance Overview

| Strategy | Source | Total Return | Sharpe | Max Drawdown | Trades |
|------|------|--------|----------|----------|----------|
| 🟢 Buy & Hold | Baseline | +222.7% | 0.54 | -77.2% | 0 |
| 🟡 Z-score (30d) | NB3 | +30.9% | 0.19 | -55.3% | 21 |
| 🟡 Z-score (7d) | NB3 | +17.4% | 0.10 | -50.6% | 143 |
| 🔴 MA Cross | NB2 | -35.2% | -0.24 | -76.4% | 362 |
| 🔴 Bollinger Bands | NB2 | -42.8% | -0.27 | -66.8% | 418 |
| 🔴 RSI | NB2 | -77.0% | -0.68 | -84.2% | 388 |
| 🔴 XGBoost | NB4 | -33.9% | -1.29 | -43.3% | — |

> Includes 0.1% fee + 0.05% slippage. NB4 test set only (2023.10-2024.12).

### 3. Three Counter-Intuitive Conclusions

**Stop-loss doesn't always protect you**  
In BTC's high-volatility environment, both fixed and trailing stop-losses increased losses due to frequent triggers.

**Trading frequency is a hidden killer**  
Within the same framework, 21 trades (Sharpe 0.19) vastly outperformed 143 trades (Sharpe 0.10).

**ML underperforms simple rules**  
XGBoost's top features (ma_ratio_24, zscore_168) matched hand-crafted rules exactly, yet couldn't outperform them.

## 📂 Project Structure

```mermaid
graph TD
    R["crypto-trading-research/"]:::root
    R --> N["📓 notebooks/"]:::nb
    R --> D["💾 data/"]:::data
    R --> S["📄 requirements.txt"]:::file
    R --> M["📖 README.md"]:::file

    N --> N1["01 Data Pipeline"]:::nb
    N --> N2["02 Classic Indicators"]:::nb
    N --> N3["03 Statistical Strategy"]:::nb
    N --> N4["04 ML Signals"]:::nb
    N --> N5["05 Backtest Engine"]:::nb
    N --> N6["06 Performance Analysis"]:::nb
    N --> N7["07 Conclusions"]:::nb

    D --> D1["config.json"]:::file
    D --> D2["*.parquet (Run NB1 to generate)"]:::gen

    classDef root fill:#eaecee,stroke:#5d6d7e,color:#2c3e50,stroke-width:2px
    classDef nb fill:#e8daef,stroke:#7d3c98,color:#4a235a,stroke-width:1px
    classDef data fill:#d1ecf1,stroke:#0c7ea8,color:#0c5a77,stroke-width:1px
    classDef file fill:#eaecee,stroke:#5d6d7e,color:#2c3e50,stroke-width:1px
    classDef gen fill:#fef9e7,stroke:#d4ac0d,color:#7d6608,stroke-width:1px
```

## 🚀 Quick Start

### Requirements
- Python 3.10+
- Access to Binance API (proxy may be needed in some regions)

### Setup

```bash
git clone <this repo>
cd crypto-trading-research
pip install -r requirements.txt
cd notebooks
jupyter notebook
```

> ⚠️ Run `01_数据获取与清洗.ipynb` first to generate data. All subsequent notebooks depend on it.  
> ⚠️ If you need a proxy for Binance, update the proxy config in NB1.

## 🧭 Methodology

```mermaid
graph LR
    A["💭 Hypothesis"]:::step -->|"Design strategy"| B["🧪 Validate"]:::step
    B -->|"Backtest"| C["🔍 Analyze"]:::step
    C -->|"Find issues"| D["🔄 Iterate"]:::step
    D -->|"New hypothesis"| A

    classDef step fill:#e8daef,stroke:#7d3c98,color:#4a235a,stroke-width:2px
```

Each notebook starts with a **decision log** documenting key choices and rationale:

| Notebook | Hypothesis | Result |
|----------|----------|----------|
| NB2 | Classic indicators capture BTC trends | ❌ All lost money — lagging signals + high volatility = frequent false breakouts |
| NB3 | Price reverts after deviation from mean | ✅ Z-score achieved first positive return, but low time-in-market |
| NB4 | ML multi-factor improves prediction accuracy | ❌ Severe overfitting, test accuracy ≈ random guess |
| NB5 | Slippage significantly impacts returns | ✅ Slippage cut Z-score returns nearly in half |

## 📊 Tech Stack

| Tool | Usage |
|------|------|
| ccxt | Exchange API for fetching candle data |
| pandas / numpy | Data processing and computation |
| matplotlib / seaborn | Visualization |
| scipy | Statistical analysis (normality tests, QQ plots) |
| XGBoost | Machine learning model |
| scikit-learn | Model evaluation, time series split |

## 📝 License

MIT License
