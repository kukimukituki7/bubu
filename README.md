# BTC Chaos Dual-Mode Engine

A regime-aware Bitcoin trading indicator built using chaos theory principles,
market microstructure, and nonlinear statistics.

This system is **Bitcoin-only** and designed for **BTCUSDT perpetuals**.
It avoids prediction and instead classifies **market state** before trading.

---

## Core Idea

Bitcoin price action is **deterministic but chaotic**, not random.

The goal is not to predict price, but to:
1. Detect whether the market is in **order (trend)** or **chaos (liquidity sweep)**
2. Apply the **correct trading logic for that regime**

Single-mode systems fail in BTC.
This engine uses **dual-mode logic**.

---

## Regimes Detected

### 1. Trend / Impulse Regime
Characteristics:
- High directional efficiency
- Volatility expansion with follow-through
- EMA structure aligned
- Small wicks, large bodies

Expectation:
➡ Continuation

### 2. Sweep / Mean-Reversion Regime
Characteristics:
- Volatility spikes without persistence
- Large wicks relative to bodies
- Structure breaks that immediately reject
- High entropy / low efficiency

Expectation:
➡ Snapback / fade

---

## Chaos & Regime Metrics Used

This is **not EMA-only** logic.

The classifier uses:

- **Directional Efficiency Ratio**  
  Measures how cleanly price moves in one direction  
  (trend = low entropy, chop = high entropy)

- **Wick Dominance Ratio**  
  Detects liquidity hunts and stop runs  
  (large wicks = sweep behavior)

- **Range Persistence**  
  Distinguishes real expansion from one-bar liquidation events

- **EMA Structure**  
  Used as a secondary structural filter, not as a predictor

---

## Signals & Their Meaning

### 🔺 Triangles = Trend-Following Signals

Only appear when the market is classified as **Trend Regime**.

- **Green ▲** → Trend BUY  
- **Red ▼** → Trend SELL / Short  

Trade logic:
- Hold winners
- Trail stops
- Expect continuation

---

### ⚪ Circles = Mean-Reversion (Sweep) Signals

Only appear when the market is classified as **Sweep / Chaos Regime**.

- **Light Green ●** → Mean-Reversion BUY  
- **Purple / Maroon ●** → Mean-Reversion SELL  

Trade logic:
- Fade extremes
- Smaller targets
- Faster exits

---

## Critical Rule

**Do not mix signal types.**

Triangles and circles are:
- Different regimes
- Different expectations
- Different risk models

Trading them the same way defeats the system.

---

## Recommended Usage

- **Symbol:** BTCUSDT.P (perpetual)
- **Primary timeframe:** 1H
- **Optional filter:** 4H regime alignment
- **Execution:** After candle close

This is a **regime engine**, not a scalper or a prediction tool.

---

## Philosophy

Chaos theory does not predict outcomes.
It identifies **when prediction is possible**.

This system trades **order emerging from chaos** —
and steps aside when chaos dominates.

---

## Next Possible Extensions

- Strategy version with backtesting
- Background regime coloring
- Higher-timeframe regime filter
- Fractal dimension / Hurst exponent proxies
- Volume & liquidation-based filters

---

## Disclaimer

This indicator is a research tool.
No indicator removes risk.
Always validate across multiple BTC market cycles.
