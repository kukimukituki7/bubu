# Vector Extreme - Complete Trading Guide

## Overview

Vector Extreme is a comprehensive TradingView indicator combining three powerful market analysis tools:
1. **Watson** (NWE Envelope) - Dynamic support/resistance bands
2. **Vector Candles** (PVSRA) - Volume-based candle coloring
3. **OI Delta** - Open Interest flow analysis with liquidation levels

---

## 1. Watson (NWE Envelope)

### What It Is
Watson uses a Gaussian-weighted regression to create smooth, adaptive envelope bands around price. It identifies the "normal" price range and highlights deviations.

### Settings
| Setting | Default | Description |
|---------|---------|-------------|
| Bandwidth | 8 | Smoothing factor (higher = smoother) |
| Mult | 3 | Distance of bands from center |
| Source | close | Price source for calculation |
| Repainting | true | Enable/disable repainting mode |

### How To Use
- **Price near upper band**: Potential resistance / overextended up
- **Price near lower band**: Potential support / overextended down
- **▲ arrow**: Price crosses below lower band = BUY signal
- **▼ arrow**: Price crosses above upper band = SELL signal
- Best used in ranging markets for mean reversion

---

## 2. Vector Candles (PVSRA)

### What It Is
PVSRA (Price Volume Spread Analysis) colors candles based on volume characteristics to reveal institutional activity.

### Candle Colors
| Color | Meaning |
|-------|---------|
| 🟢 **Lime** | Climax up - high volume + price up (exhaustion?) |
| 🔴 **Red** | Climax down - high volume + price down (exhaustion?) |
| 🔵 **Blue** | Above average up - strong bullish volume |
| 🟣 **Fuchsia** | Above average down - strong bearish volume |
| ⚪ **Gray** | Normal volume - no significant activity |

### How To Use
- **Climax candles (lime/red)**: Often mark tops/bottoms - be cautious
- **Above average (blue/fuchsia)**: Institutional buying/selling - trend continuation
- **Normal (gray)**: Low conviction - avoid trading
- Color transitions (e.g., lime→red) often signal trend reversals

---

## 3. OI Delta (Open Interest Flow)

### What It Is
OI Delta tracks open interest changes combined with price action to identify who is entering positions (buyers vs sellers) and potential liquidation zones.

### Flow Classification
| Condition | Meaning | Arrow Color |
|-----------|---------|-------------|
| **newLongs** | Price UP + OI UP | 🟢 Green |
| **newShorts** | Price DOWN + OI UP | 🔴 Red |
| **shortsClosing** | Price UP + OI DOWN | 🟣 Purple |
| **longsClosing** | Price DOWN + OI DOWN | 🟠 Orange |
| **chopOrAbsorb** | OI change <10% of price | ⚪ Gray X |

### Liquidation Levels
| Line | Color | Meaning |
|------|-------|---------|
| Long Entry | 🟢 Green | Price where longs entered |
| Short Entry | 🔴 Red | Price where shorts entered |
| Long Liq | 🟠 Orange | Long liquidation level (entry - X%) |
| Short Liq | 🟣 Purple | Short liquidation level (entry + X%) |
| Swing Liq | Thicker lines | Liq levels based on swing highs/lows |

### Settings
| Setting | Default | Description |
|---------|---------|-------------|
| Liq % | 1% | Liquidation percentage distance |
| Show Price Entry Liqs | On | Show entry-based liq levels |
| Show Swing Liqs | On | Show swing-based liq levels |
| Swing Lookback | 5 | Bars for swing high/low |
| OI Offset | 2000 | Vertical offset for OI plot |

### How To Use
- **Green arrow (newLongs)**: New buyers entering - bullish flow
- **Red arrow (newShorts)**: New sellers entering - bearish flow
- **Purple arrow (shortsClosing)**: Shorts covering - potential squeeze
- **Orange arrow (longsClosing)**: Longs liquidating - potential drop
- **Gray X (chop)**: Low OI change = no clear direction - avoid
- **Yellow zone**: Longs underwater (price below entry)
- **Blue zone**: Shorts underwater (price above entry)

---

## Combined Trading Strategy

### Entry Rules

**LONG Setup:**
1. OI shows `newLongs` (green arrow) - bullish flow
2. NOT during chop (no gray X)
3. Optional: Price near Watson lower band (oversold)
4. Optional: Vector candle is blue/fuchsia (institutional buying)

**SHORT Setup:**
1. OI shows `newShorts` (red arrow) - bearish flow
2. NOT during chop (no gray X)
3. Optional: Price near Watson upper band (overbought)
4. Optional: Vector candle is fuchsia (institutional selling)

### Exit Rules
- **Take Profit**: Opposite OI signal (e.g., newShorts closes LONG position)
- **Stop Loss**: At liquidation level (orange for long, purple for short)
- **Time Exit**: If underwater zone persists >5 bars, consider exit

### Risk Management
- Default liquidation %: 1% (adjust based on leverage)
- Use swing liqs for wider stops in volatile markets
- Reduce position size when chop (gray X) is frequent

---

## Data Sources

| Data | Source |
|------|--------|
| Price + OI | BINANCE:BTCUSDT.P |
| OI | BINANCE:BTCUSDT.P_OI |
| Longs/Shorts | BITFINEX:BTCUSDLONGS / BTCUSDSHORTS |

---

## Tips

1. **All three tools agree = higher probability**: e.g., Watson oversold + newLongs + blue vector candle
2. **Climax + opposite OI = reversal**: Red climax candle + newShorts = strong short
3. **Watch underwater zones**: When longs are underwater (yellow), price often bounces from liq level
4. **Chop is your friend**: When you see gray X, don't force trades - wait for clarity
5. **Volume confirms direction**: Blue/fuchsia candles + OI flow in same direction = stronger signal
