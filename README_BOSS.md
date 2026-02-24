# BOSS Indicator - Complete Trading Guide

## Overview

BOSS (Build Ordinary Simple System) is a comprehensive multi-component trading indicator combining:
1. **Chaos Dual** - EMA ribbon with trend/mean reversion regime detection
2. **Supply/Demand Zones** - 4 RSI-based S/D zone modules
3. **Swing Structure** - HH/HL/LH/LL swing labeling
4. **CVD Intensity** - Lower timeframe volume intensity signals
5. **CVD Divergence** - Cumulative Volume Delta divergence detection

---

## 1. Chaos Dual (EMAs + Regime)

### What It Is
A dual-mode trading system that detects whether the market is in **trend** or **sweep** (mean reversion) mode, then provides appropriate signals.

### Components

#### EMA Ribbon
| EMA | Color | Purpose |
|-----|-------|---------|
| EMA 21 | Teal | Fast trend |
| EMA 55 | Orange | Mid trend |
| EMA 100 | Blue | Slow trend |
| EMA 200 | Red | Long-term trend |

### Regime Detection

**Trend Mode** (when ALL conditions met):
- Directional Efficiency > 0.4 (price moves efficiently in one direction)
- Range Persistence >= 3 (large ranges in 5 bars)
- NOT in sweep (no wick dominance)

**Sweep Mode**:
- Wick dominance > 2 (wicks dominate over bodies)
- OR low directional efficiency

### Signals

| Signal | Shape | Color | Condition |
|--------|-------|-------|-----------|
| **Trend Buy** | ▲ Triangle | Green | Trend mode + bull EMAs + price above EMA55 |
| **Trend Sell** | ▼ Triangle | Red | Trend mode + bear EMAs + price below EMA55 |
| **MR Buy** | ● Circle | Lime | Sweep mode + price below EMA21 + wickRatio > 2 |
| **MR Sell** | ● Circle | Maroon | Sweep mode + price above EMA21 + wickRatio > 2 |

### How To Use
- **Trend mode**: Follow the trend, use EMA crossovers for entries
- **Sweep mode**: Mean reversion trades, expect bounces off EMA21
- **Green triangles**: Strong trend continuation
- **Green circles**: Potential reversal/sweep trades

---

## 2. Supply/Demand Zones (4 Modules)

### What It Is
Four independent RSI-based supply/demand zone generators. Each tracks when RSI stays in overbought/oversold territory for X consecutive bars, then marks the high/low of that zone.

### Settings Per Module
| Setting | Module 1 | Module 2 | Module 3 | Module 4 |
|---------|----------|----------|----------|----------|
| RSI Length | 7 | 14 | 21 | 28 |
| Default OB/OS | 70/30 | 70/30 | 70/30 | 70/30 |
| Default | OFF | ON | OFF | OFF |

### Zone Types
| Zone | Shows | Color (default) |
|------|-------|-----------------|
| **Supply/Demand** | Blue zone at RSI extreme | Blue (85% transp) |
| **Support Zone** | Green zone when RSI oversold | Green (92% transp) |
| **Resistance Zone** | Red zone when RSI overbought | Red (92% transp) |

### How To Use
- **Price enters S/D zone**: Potential reversal area
- **Support zone (green)**: Buyers stepping in at oversold RSI
- **Resistance zone (red)**: Sellers stepping in at overbought RSI
- **Multiple zones aligning**: Higher probability reversal

---

## 3. Swing Structure (HH/HL/LH/LL)

### What It Is
Labels swing highs and lows based on RSI overbought/oversold transitions.

### Labels
| Label | Meaning | Color |
|-------|---------|-------|
| **HH** | Higher High | Red (above bar) |
| **LH** | Lower High | Red (above bar) |
| **HL** | Higher Low | Green (below bar) |
| **LL** | Lower Low | Green (below bar) |

### How To Use
- **HH/LH**: Potential resistance levels
- **HL/LL**: Potential support levels
- **Trend continuation**: HH + HL (higher highs + higher lows)
- **Trend reversal**: LH + LL (lower highs + lower lows)

---

## 4. CVD Intensity (Lower Timeframe)

### What It Is
Aggregates lower timeframe (default: 1min) volume × body × direction to show who is in control.

### Signals
| Signal | Shape | Color | Meaning |
|--------|-------|-------|---------|
| **Buy** | ▲ Triangle | Green | Intensity flips from negative to positive |
| **Sell** | ▼ Triangle | Red | Intensity flips from positive to negative |

### Settings
- **Lower Timeframe**: 1 (default)
- **Lookback**: 10 bars

### How To Use
- **Green triangle**: Buyers aggressive - potential long
- **Red triangle**: Sellers aggressive - potential short
- Use with regime for confirmation

---

## 5. CVD Divergence

### What It Is
Tracks Cumulative Volume Delta (CVD) and detects divergences between price and volume flow.

### Components

**CVD Calculation**:
- Aggregates lower timeframe (default: 1min) volume with directional bias
- Resets on daily basis (configurable)

**Bullish Divergence**:
- Price makes new low BUT
- CVD makes higher low (less selling pressure)
- Green circle marks the divergence

**Bearish Divergence**:
- Price makes new high BUT
- CVD makes lower high (less buying pressure)
- Red circle marks the divergence

### How To Use
- **Green circle (below price)**: Bullish divergence - potential long
- **Red circle (above price)**: Bearish divergence - potential short
- Divergence + sweep signal = high probability setup

---

## Combined Trading Strategy

### Setup 1: Trend Continuation
1. Green triangle (trend buy) appears
2. Price above EMA 55
3. EMA ribbon stacked (21 > 55 > 100 > 200)
4. Enter on next bar

### Setup 2: Mean Reversion Sweep
1. Green circle (MR buy) appears
2. Price near support (HL/LL zone)
3. CVD showing bullish divergence
4. Enter on bounce

### Setup 3: Bearish Reversal
1. Red circle (MR sell) appears
2. Price below EMA 21 with wick dominance
3. Resistance zone (red) above
4. Enter on rejection

### Setup 4: Volume Confirmation
1. Any entry signal
2. CVD intensity shows same direction (green triangle for longs)
3. Divergence aligns (bullish for longs)
4. Higher probability trade

---

## Legend Summary

### Chaos Dual Signals
| Symbol | Name | Meaning |
|--------|------|---------|
| ▲ (green) | Trend Buy | Strong trend entry |
| ▼ (red) | Trend Sell | Strong trend entry |
| ● (lime) | MR Buy | Sweep/reversal long |
| ● (maroon) | MR Sell | Sweep/reversal short |

### Swing Labels
| Label | Full Name | Direction |
|-------|-----------|-----------|
| HH | Higher High | Bullish continuation |
| LH | Lower High | Bearish reversal |
| HL | Higher Low | Bullish reversal |
| LL | Lower Low | Bearish continuation |

### CVD Intensity
| Signal | Meaning |
|--------|---------|
| ▲ (green) | Aggressive buying |
| ▼ (red) | Aggressive selling |

### CVD Divergence
| Circle | Meaning |
|--------|---------|
| Green (below) | Bullish divergence |
| Red (above) | Bearish divergence |

---

## Default Settings

### Supply/Demand (all modules)
- Module 1: OFF (RSI 7)
- Module 2: ON (RSI 14) - RECOMMENDED
- Module 3: OFF (RSI 21)
- Module 4: OFF (RSI 28)

### Swing Structure
- RSI Length: 7
- Overbought: 70
- Oversold: 30

### CVD Intensity
- Lower TF: 1
- Lookback: 10

### CVD Divergence
- Lower TF: 1
- Reset: Daily

---

## Tips

1. **Start with Module 2 only** - RSI 14 is the most reliable
2. **Combine indicators** - Don't trade on single signal
3. **Trend + Sweep** - Use EMAs to filter direction, then trade sweeps within trend
4. **Volume confirms** - CVD signals should align with price direction
5. **Divergences are powerful** - When price/CVD diverge, expect reversal
