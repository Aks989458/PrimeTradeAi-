# Crypto Market Sentiment & Trader Performance Analysis

## Overview

This project analyzes the relationship between Bitcoin market sentiment and trader performance using:

1. Bitcoin Fear & Greed Index
2. Historical Hyperliquid Trader Data

The objective is to:

- Understand how market sentiment affects trader profitability
- Identify profitable trading behaviors
- Build a machine learning model to predict high-profit trades
- Generate actionable insights for smarter trading strategies

---

# Dataset Information

## 1. Fear & Greed Dataset

Contains daily Bitcoin market sentiment.

### Columns

| Column | Description |
|---|---|
| date | Date |
| classification | Market sentiment category |
| value | Numerical sentiment score |

### Sentiment Classes

- Extreme Fear
- Fear
- Neutral
- Greed
- Extreme Greed

---

## 2. Historical Trader Dataset

Contains historical trade execution data from Hyperliquid.

### Important Columns

| Column | Description |
|---|---|
| account | Trader wallet/account |
| coin | Asset traded |
| execution_price | Trade execution price |
| size_tokens | Quantity traded |
| size_usd | Trade value in USD |
| side | BUY/SELL |
| direction | Trade direction |
| closed_pnl | Realized PnL |
| fee | Trading fee |
| timestamp_ist | Trade timestamp |

---

# Project Workflow

## 1. Data Collection

Datasets are downloaded automatically using `gdown`.

---

## 2. Data Cleaning

Performed:
- Duplicate removal
- Missing value handling
- Numeric type conversion
- Invalid value removal
- Timestamp correction

---

## 3. Data Merging

Merged trader data with sentiment data using:
- standardized daily timestamps

This enabled sentiment-aware trade analysis.

---

# Exploratory Data Analysis (EDA)

The following analyses were performed:

## Market Sentiment Distribution
- Frequency of Fear/Greed conditions

## PnL Distribution
- Distribution of trader profitability

## Sentiment vs Profitability
- Impact of market sentiment on PnL

## Trade Size vs Profitability
- Relationship between position size and returns

## Coin-wise Performance
- Most profitable traded assets

## Correlation Analysis
- Relationships between engineered features

---

# Statistical Analysis

## Skewness & Kurtosis

The dataset showed:
- heavy-tailed distributions
- extreme outliers
- whale trading behavior

### Observations

| Feature | Observation |
|---|---|
| size_tokens | Extremely skewed |
| closed_pnl | Heavy-tailed |
| fee | Large outliers |

---

# Outlier Handling

Outliers were handled using:
- 1st percentile clipping
- 99th percentile clipping

This improved:
- model stability
- generalization
- robustness

---

# Feature Engineering

Several predictive features were engineered.

## Sentiment Features

| Feature | Description |
|---|---|
| sentiment_encoded | Numerical encoding of sentiment |

---

## Trading Features

| Feature | Description |
|---|---|
| is_buy | Buy/Sell encoding |
| fee_ratio | Fee relative to trade size |
| exposure | Position exposure |
| direction_encoded | Encoded trade direction |

---

## Log-Transformed Features

To reduce skewness:

- size_tokens_log
- size_usd_log
- closed_pnl_log
- avg_pnl_log
- total_pnl_log

---

# Target Variable

The target variable predicts:

## High-Profit Trades

Trades above the 75th percentile of profitability were labeled:

| Target | Meaning |
|---|---|
| 1 | High-profit trade |
| 0 | Low-profit trade |

This creates a stronger predictive framework than simple positive/negative PnL classification.

---

# Machine Learning Model

## Model Used

### XGBoost Classifier

Chosen because:
- excellent for tabular financial data
- robust to nonlinear relationships
- handles skewed distributions effectively
- strong performance on structured datasets

---

# Leakage Prevention & Validation

Several techniques were used to ensure realistic and trustworthy performance.

## 1. Removed Leakage Features

Removed:
- win_rate
- avg_pnl
- total_pnl
- pnl_std

These contained future information.

---

## 2. Chronological Train-Test Split

Used time-based splitting instead of random splitting to prevent future data leakage.

---

## 3. Class Imbalance Handling

Used:
- `scale_pos_weight`

inside XGBoost.

---

## 4. Robust Scaling

Applied:
- `RobustScaler`

to reduce sensitivity to outliers.

---

## 5. Cross Validation

Performed 5-fold cross validation using ROC-AUC scoring.

---

## 6. Shuffle Test

A shuffled-target leakage test was conducted.

Purpose:
- verify model learns real patterns
- detect hidden leakage

---

## 7. Duplicate Leakage Check

Checked:
- exact duplicate rows
- feature-level duplicates

---

# Model Performance

## Final Metrics

| Metric | Score |
|---|---|
| Accuracy | ~95% |
| F1 Score | ~0.91 |
| ROC-AUC | ~0.99 |

After leakage prevention and time-based validation:
- metrics became more realistic and trustworthy.

---

# Feature Importance

Top predictive features included:

- sentiment_encoded
- exposure
- size_usd_log
- fee_ratio
- execution_price

This indicates:
- market sentiment strongly impacts profitability
- risk exposure significantly influences outcomes

---

# SHAP Explainability

SHAP was used for:
- model interpretability
- understanding feature impact
- explaining predictions

This improved transparency of the trading model.

---

# Key Insights

## 1. Market Sentiment Influences Profitability

Greed periods generally showed:
- larger profitable trades
- increased trading activity

Fear periods showed:
- higher volatility
- unstable returns

---

## 2. Large Trades Create Heavy-Tailed Risk

A small number of trades dominate total profitability.

---

## 3. Trader Exposure Matters

Higher exposure:
- increases potential returns
- significantly increases volatility

---

## 4. Fees Reduce Profitability

Fee-heavy strategies consistently underperformed.

---

## 5. Sentiment-Aware Trading Is Valuable

Combining:
- sentiment indicators
- trade behavior

improves predictive performance significantly.

---

# Technologies Used

| Tool | Purpose |
|---|---|
| Python | Programming |
| Pandas | Data Processing |
| NumPy | Numerical Computing |
| Matplotlib | Visualization |
| Seaborn | Statistical Visualization |
| Scikit-learn | ML Utilities |
| XGBoost | Machine Learning |
| SHAP | Explainability |

---

# Installation

```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost shap gdown
```

---

# Run the Project

```bash
python main.py
```

Or run the notebook sequentially.

---



# Conclusion

This project demonstrates how:
- market sentiment
- trader behavior
- risk exposure

can be combined to build an explainable AI-driven trading analytics framework.

The system successfully:
- identifies profitable patterns
- predicts high-profit trades
- provides actionable trading insights
- validates model robustness using anti-leakage techniques

---
