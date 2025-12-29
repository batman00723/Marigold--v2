# Marigold V2 — Trade vs No-Trade Signal Validation

**Marigold** is a long-term project to build a robust ML-driven crypto trading system.  
This repo contains **V2**, focused strictly on **decision quality**, not profitability.

> **Core question:**  
> *Is this moment worth trading, or should we stay out?*

---

## What This Project Shows (for recruiters)

- Correct problem framing (trade vs no-trade, not price prediction)
- Honest, future-aware labeling (no leakage)
- Disciplined feature selection (no indicator soup)
- Stable evaluation across time splits
- Regime-aware analysis (volatility matters)

This is **system design + ML thinking**, not metric chasing.

---

## Data

- **Asset:** BTC/USDT  
- **Timeframe:** 2-hour candles  
- **Period:** ~2021–2025  
- **Source:** Binance (CCXT)  
- **Split:** Time-based  
  - Train 60% / Val 20% / Test 20%  
  - No shuffling

---

## Target (Key Contribution)

Binary label: **Trade vs No-Trade**

A trade is **rejected** if it drops too much *before* rising, even if it later goes up.

**Volatility-aware asymmetric rule (5-candle horizon):**
if worst_drop < -0.6 × vol_30:
no trade
elif best_rise >= +1.0 × vol_30:
trade
else:
no trade


- `vol_30` = rolling std of past 30 returns  
- Capital preservation explicitly enforced  
- Last 5 candles unlabeled (no future)

**Resulting class balance:** ~21% trade / ~79% no-trade (stable across splits)

---

## Features (Final, Frozen)
return_1 # short-term impulse
vol_30 # volatility regime
momentum_10 # directional bias
range_position_10 # position inside recent range


Tested and **discarded** features that didn’t add value.

---

## Model (Diagnostic Only)

- **DecisionTreeClassifier (max_depth=3)**
- Used to inspect structure, not to optimize performance

**ROC-AUC (expected for financial data):**
- Validation ≈ 0.52  
- Test ≈ 0.51–0.52  
Stable, weak, honest signal.

---

## Key Insight

Signal performs **slightly better in high-volatility regimes**, indicating it acts as an **impulse / expansion filter** rather than a calm-market predictor.

---

## Status

- ✅ V2 complete and frozen  
- ❌ Not a profitable strategy (by design)

**Next:** V3 focuses on **paper trading & execution logic**, treating V2 as a black-box signal.

---

## Takeaway

This project prioritizes **clear thinking, correct labels, and system behavior** over chasing metrics.  
It reflects how real ML systems are designed before deployment.

