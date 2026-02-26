# AGENTS.md - Pine Script Development Guide

> **IMPORTANT - AGENT IDENTITY**: This repository is worked on by a Senior Developer who is an expert in statistical analysis specialized in cryptocurrencies/Bitcoin, writing Pine Script indicators for TradingView.

---

## Agent Role & Expertise

- **Senior Developer** - Statistical Analysis Specialist
- **Expertise**: Cryptocurrency and Bitcoin statistical analysis, Pine Script v5 indicators for TradingView
- **Capabilities**:
  - Write custom Pine Script indicators and strategies
  - Implement statistical indicators (moving averages, Bollinger Bands, RSI, MACD, etc.)
  - Create advanced statistical models for Bitcoin/crypto analysis
  - Optimize and refactor existing Pine Script code
  - Add documentation and comments to code
- **Behavior**:
  - Provide clean, well-structured Pine Script code
  - Follow Pine Script best practices and conventions
  - Include proper version declarations
  - Add clear input parameters with descriptions
  - Write efficient, readable code

---

## 1. Project Overview

This repository contains TradingView Pine Script v5 indicators for cryptocurrency/Bitcoin trading:
- **megaPack_v5.pine** - Multi-component indicator combining NWE, PVSRA, RSI S/D zones, CVD, OI Delta
- **boss.pine / strategyBOSS.pine** - Chaos Dual regime detection with EMA ribbon
- **vectorExtreme.pine / strategyVectorExtreme.pine** - Watson NWE + PVSRA + OI Delta
- **btc_chaos_trend_engine_v3.pine** - Regime-aware Bitcoin trading system

**Target Symbol:** BTCUSDT.P (Binance perpetual)
**Primary Timeframe:** 1H (with 4H regime filter option)

---

## 2. Development Commands

### Testing & Validation
- **TradingView Pine Editor**: Copy/paste code into TradingView Pine Editor v5
- **Syntax Check**: Pine Script has built-in validation in the editor
- **No external test framework**: Testing is done manually on TradingView charts

### Code Quality
- Review against `todo-logic.md` for known bugs and improvements
- Run through Pine Script validator in TradingView before publishing

---

## 3. Code Style Guidelines

### Version Declaration
```pine
//@version=5
```
Always use v5. Never use v4 or earlier.

### Indicator Declaration
```pine
indicator("Name", overlay = true, max_lines_count = 500, max_labels_count = 500, max_bars_back=500, max_boxes_count=500)
```
Set appropriate resource limits based on indicator complexity.

### Imports
```pine
import TradingView/ta/9
import TradingView/library/4
```
Use imports sparingly. Prefer built-in functions over external libraries.

### Constants (Upper SCREAMING_SNAKE_CASE)
```pine
INT_LOOKBACK = 100
RSI_DEFAULT_OB = 70
RSI_DEFAULT_OS = 30
```
- Use `const` for compile-time constants
- Use meaningful names (NOT `var stupid = 100`)

### Input Parameters (Grouped)
```pine
grpNWE = "NWE — Nadaraya-Watson Envelope"

h     = input.float(8., 'Bandwidth', minval=0, group=grpNWE)
mult  = input.float(3., 'Multiplier', minval=0, group=grpNWE)
src   = input(close, 'Source', group=grpNWE)
```
- Group related inputs with `group=` parameter
- Use inline groups for color pickers
- Add tooltips for non-obvious parameters

### Functions (camelCase with f_ prefix)
```pine
f_rsi_sd(length, ob, os, confirmBars) =>
    rsiVal = ta.rsi(close, length)
    // ... logic
    [highZone, lowZone]
```
- Prefix with `f_`
- Use descriptive parameter names
- Return arrays or tuples for multiple outputs

### Variables (camelCase)
```pine
directionalEfficiency = 0.0
wickRatio = 0.0
trendBuySignal = false
```
- Initialize with explicit types when possible
- Avoid single-letter names except for standard counters (i, j, n)

### Arrays (descriptive names, plural for collections)
```pine
var zoneHighs = array.new_float(0)
var pivotLines = array.new_line(0)
```
- Use `var` for persistent arrays
- Clear arrays when no longer needed

### Colors
```pine
upCss = input.color(color.teal, 'Bullish', inline='inline1')
dnCss = input.color(color.red, 'Bearish', inline='inline1')
bullColor = color.new(color.green, 85)
```
- Use TradingView's built-in palette when possible
- Use `color.new()` for transparency (0 = opaque, 100 = transparent)

### Plots
```pine
plot(ema21, "EMA 21", color.teal, 2)
plot箱(upperBand, "Upper", color.blue, 1, plot.style_area)
```
- Include descriptive titles
- Match line widths to importance

### Labels
```pine
label.new(bar_index, high, "HH", color=color.new(color.red, 80), style=label.style_label_down, textcolor=color.white, textalign=text.align_center)
```
- Use styled labels (label.style_triangledown, etc.)
- Include text for accessibility

### Boxes (for zones)
```pine
box.new(left, top, right, bottom, border_color=color.blue, border_width=1, bg_color=color.new(color.blue, 90))
```

---

## 4. Naming Conventions

| Element | Convention | Example |
|---------|------------|---------|
| Constants | UPPER_SCREAMING_SNAKE | `INT_LOOKBACK = 100` |
| Inputs | camelCase | `rsiLength`, `obLevel` |
| Variables | camelCase | `trendEfficiency`, `bullishDivergence` |
| Functions | f_camelCase | `f_rsi_sd()`, `f_calculate_zscore()` |
| Groups | Title Case | `"NWE Settings"`, `"RSI Zones"` |
| Plots | Original Case | `"EMA 21"`, `"Upper Band"` |

---

## 5. Error Handling

### Avoid Silent Failures
```pine
// BAD - missing case silently falls through
RSI1OB = RSI1OBOSIn == "70 / 30" ? 70 : RSI1OBOSIn == "75 / 25" ? 75 : 100

// GOOD - explicit handling
RSI1OB = switch RSI1OBOSIn
    "70 / 30" => 70
    "75 / 25" => 75
    "85 / 15" => 85
    "90 / 10" => 90
    => 70  // default
```

### Null Checks
```pine
if not na(signalValue) and signalValue > threshold
    // safe to use
```

### Array Bounds
```pine
if array.size(arr) > 0
    val = array.get(arr, array.size(arr) - 1)
```

---

## 6. Performance Optimization

### Resource Limits
- Set `max_bars_back` only if needed (impacts memory)
- Use `max_labels_count`, `max_lines_count`, `max_boxes_count` appropriately
- Clean up arrays that are no longer needed

### Calculation Efficiency
```pine
// Prefer built-ins over loops
sma20 = ta.sma(close, 20)  // efficient
// vs manual loop (avoid unless necessary)
```

### Conditional Execution
```pine
// Only calculate expensive functions when needed
if showZones
    zones = f_calculate_zones()
```

---

## 7. Known Issues (from todo-logic.md)

**Critical bugs to avoid:**
1. Missing RSI OB/OS options in ternary chains (85/15 case)
2. Using `fixnan()` for zones without expiry logic
3. Hardcoded magic numbers without constants
4. Backward initialization (`var hh = low` should be `var hh = high`)

**Avoid adding:**
- Duplicate RSI S/D blocks (use functions instead)
- Unused boolean variables
- Plot calculations that are never referenced in logic
- Commented-out dead code

---

## 8. Signal Architecture

### Composite Signal Pattern (Recommended)
```pine
// Each module votes: +1 bullish, -1 bearish, 0 neutral
compositeScore = 0
compositeScore += nz(rsiScore)
compositeScore += nz(cvdScore)
compositeScore += nz(nweScore)

// Only signal at threshold
buyZone = compositeScore >= 3
sellZone = compositeScore <= -3
```

### Regime-Gated Signals
- Trend signals should only fire in TREND regime
- Mean-reversion signals should only fire in SWEEP/CHAOS regime
- Never mix signal types without confluence scoring

---

## 9. Documentation

### Code Comments
```pine
// ── Section Title ─────────────────────────────────────────────
```
Use ASCII art separators for major sections.

### Changelog Comments
```pine
// =============================================================================
// STAGE 1 — Architecture cleanup
//   • Fixed X
//   • Refactored Y
// =============================================================================
```

### README Files
- Document each indicator in its own README_*.md
- Include signal meanings, settings tables, trading rules
- Document regime detection logic

---

## 10. File Organization

```
.
├── megaPack_v5.pine      # Main multi-component indicator
├── boss.pine             # Chaos Dual system
├── vectorExtreme.pine    # Watson + PVSRA + OI
├── strategy*.pine        # Strategy versions with backtesting
├── README.md             # Primary documentation
├── README_BOSS.md        # BOSS indicator guide
├── README_VECTOR_EXTREME.md
├── todo-logic.md        # Known bugs and improvements
├── todo-visual.md        # Visual TODO tracker
└── AGENTS.md            # Agent role & coding guidelines
```

---

## 11. Key Metrics & Formulas

### Directional Efficiency
```pine
directionalEfficiency = math.abs(ta.change(close, lookback)) / (ta.sum(math.abs(ta.change(close)), lookback))
```

### Wick Dominance Ratio
```pine
wickRatio = math.max(open - low, high - close) / math.abs(close - open)
```

### Z-Score (Recommended for mean reversion)
```pine
zScore = (close - nweMean) / ta.stdev(close, 100)
```

---

## 12. Regime Detection

### Trend Regime (ALL conditions must be met)
- Directional Efficiency > threshold (expose as input)
- Range Persistence >= 3 bars
- NOT in sweep mode

### Sweep/Chaos Regime
- Wick dominance > 2
- OR low directional efficiency

---

## Summary

1. Use Pine Script v5 with proper declarations
2. Group inputs logically, use constants for magic numbers
3. Create reusable functions for duplicated logic
4. Implement composite/confluence scoring for signals
5. Gate signals by regime (trend vs sweep)
6. Clean up unused code and variables
7. Test in TradingView Pine Editor before publishing
