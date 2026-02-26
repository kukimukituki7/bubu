# 🎨 todo-visual.md — MegaPack UI/UX Redesign Spec

## Current UI Score: **2.5 / 10**

### Problems (from screenshot analysis)
- 12+ overlapping elements on the main price pane at all times
- Arrows, triangles, circles, crosses, labels all competing at same visual weight
- No visual hierarchy — everything screams, nothing communicates
- RSI S/D zones in blue are indistinguishable from the NWE bands
- OI delta candles offset below price creates a visual dead zone that's neither chart nor pane
- The table/dashboard in the top-right says only "Repainting Mode Enabled" — wasted real estate
- Colors are random (lime, red, fuchsia, maroon, orange, purple all coexist on one chart)
- No glow, no depth, no modern neon aesthetic despite being a crypto tool

---

## 🎯 Design Philosophy

**Reference:** Hyperliquid, Bybit Pro charts, Axiom terminal, TradingView "Dark Neon" community themes.

**Principles:**
- Dark background (#0d0d12 or near-black charcoal) as the canvas
- Neon glow on critical signals only — not everything glows or nothing matters
- Information hierarchy: price > zones > signals > decorators
- Pane separation: keep overlay clean, push secondary data (CVD, OI, Z-Score) to sub-panes
- Consistent neon palette with purpose-coded colors (not arbitrary)

---

## 🎨 Color System (Neon Neuro Palette)

> PVSRA candle colors are locked — do not change. All other colors follow this system.

| Element | Color | Hex | Glow |
|---------|-------|-----|------|
| **Bull / Long / Buy** | Neon Cyan | `#00f5ff` | Yes, soft `#00f5ff40` |
| **Bear / Short / Sell** | Hot Magenta | `#ff00aa` | Yes, soft `#ff00aa40` |
| **Neutral / Chop** | Muted Slate | `#4a5568` | No |
| **NWE Upper Band** | Electric Teal | `#00e5cc` | Yes, `#00e5cc30` |
| **NWE Lower Band** | Coral Neon | `#ff6b6b` | Yes, `#ff6b6b30` |
| **Z-Score Extreme (long)** | Neon Green | `#39ff14` | Yes, `#39ff1450` |
| **Z-Score Extreme (short)** | Neon Red | `#ff3131` | Yes, `#ff313150` |
| **Long Liquidation** | Neon Gold | `#ffd700` | Yes, `#ffd70060` — *preserved from original* |
| **Short Liquidation** | Deep Purple Glow | `#8b00ff` | Yes, `#8b00ff60` — *preserved from original* |
| **CVD Positive** | Electric Blue | `#00aaff` | No |
| **CVD Negative** | Amber | `#ff8800` | No |
| **EMA 21** | Teal | `#00bfa5` | No |
| **EMA 55** | Soft Gold | `#ffc400` | No |
| **EMA 100** | Muted Blue | `#4d79ff` | No |
| **Pivot Rays (high)** | Faded Coral | `#ff6b6b80` | No |
| **Pivot Rays (low)** | Faded Cyan | `#00f5ff80` | No |
| **BUY ZONE label bg** | `#001a0d` | border `#39ff14` | Full glow border |
| **SELL ZONE label bg** | `#1a0007` | border `#ff00aa` | Full glow border |

---

## 📐 Layout Hierarchy (Pane Architecture)

### Main Price Pane (Overlay)
Keep **only**:
- PVSRA candle colors (locked)
- NWE bands (2 lines + fill, low opacity)
- EMA 21, 55, 100 (thin, low opacity unless in trend mode)
- Liquidation levels: Long Liq = gold dashed, Short Liq = purple dashed
- Pivot rays (thin, faded, clean)
- RSI S/D zone (ONE active zone only — the most recent)
- **Composite BUY ZONE / SELL ZONE label** (big, glowing, only fires at confluence)
- Swing HH/LL labels (tiny, clean)

Remove from main pane:
- OI delta candles (→ move to sub-pane)
- OI arrows (→ remove entirely, use pane plot)
- CVD divergence circles (→ move to CVD sub-pane)
- `chopOrAbsorb` X-cross shapes (→ remove, too noisy)
- Underwater zone bgcolors (→ replace with thinner sideline indicator)
- Redundant entry/liq steplines (simplify to just liq levels)

---

### Sub-Pane 1: Momentum (Z-Score + RSI)
- **Primary:** Z-Score line, color-coded: neon green when < -2, neon red when > +2, slate otherwise
- Background band: ±1σ = very faint fill, ±2σ = slightly brighter fill
- Reference lines at 0, ±1, ±2, ±3
- **Secondary (thin, 30% opacity):** RSI (length 14) overlaid as a faint line so the edge it adds over Z-Score is visible without dominating
- Horizontal gradient background: dark red zone above +2, dark green zone below -2
- Table inset (bottom left of pane): current Z-Score value + direction arrow

### Sub-Pane 2: CVD + OI Delta
- CVD line: electric blue above zero, amber below zero — fill to zero
- OI Delta: rendered as a histogram (not candles), same color system as main flow classification
- Zero line prominent
- CVD divergence dots moved here (bull = cyan, bear = magenta)

---

## 🖥️ Dashboard / Table Redesign

**Replace the current single-cell "Repainting Mode Enabled" table with a proper status panel.**

Position: `position.top_right`

Style:
```
Background: #0d0d1299 (semi-transparent near-black)
Border: #1a2040 (1px, subtle)
Frame: none
Font: size.small, monospace feel
```

Rows (dynamic, live-updating):
| Row | Content |
|-----|---------|
| Regime | `TREND ▲` (cyan) or `SWEEP ↔` (amber) |
| Z-Score | `-2.4 🔴 OVERSOLD` or `+0.3 ⚪ NEUTRAL` |
| CVD | `+12,450 📈` (blue) or `-8,200 📉` (amber) |
| OI Flow | `NEW LONGS` (green) / `CLOSING` (orange) etc. |
| Signal | `★ BUY ZONE` (glowing green) or `★ SELL ZONE` (glowing magenta) or `—` |

The "Repainting Mode" warning can be a small status dot (🟡 = repainting on) rather than a full cell.

---

## ✨ Glow / Neon Effect Implementation

Pine Script doesn't natively support true CSS glow, but you can simulate it:

```pine
// Simulated glow: plot same line 3x with increasing transparency
plot(buyZoneLine, color=color.new(#39ff14, 80), linewidth=4)  // outer glow
plot(buyZoneLine, color=color.new(#39ff14, 50), linewidth=2)  // mid glow  
plot(buyZoneLine, color=color.new(#39ff14, 0),  linewidth=1)  // core line
```

Use this technique for:
- NWE upper/lower bands
- Long/Short liquidation lines
- BUY ZONE / SELL ZONE composite labels (use label bgcolor + border color)
- Z-Score extreme threshold lines (±2, ±3)

---

## 🏷️ Label & Shape Design

### Composite Signal Labels (new)
```pine
// BUY ZONE label (fires only at confluence ≥ threshold)
label.new(
    bar_index, low * 0.9995,
    text      = "▲ BUY ZONE",
    style     = label.style_label_up,
    color     = color.new(#001a0d, 10),  // near-black dark green bg
    textcolor = #39ff14,                  // neon green text
    size      = size.normal,
    tooltip   = "Z:" + str.tostring(zScore, "#.##") + 
                " | CVD:" + str.tostring(cvd_ltf, "#") + 
                " | Score:" + str.tostring(compositeScore)
)
```

### Remove / Simplify These Shape Types
| Current | Replace With |
|---------|-------------|
| `plotshape(..., shape.triangleup)` × 4 duplicates | Single composite triangle, sized by score |
| `plotshape(..., shape.circle)` CVD dots | Move to CVD sub-pane |
| `plotshape(..., shape.xcross)` chop | Remove entirely |
| `plotshape(..., shape.triangledown)` × 4 duplicates | Single composite triangle |

### HH/HL/LH/LL Labels
- Keep but style upgrade: `style=label.style_none`, `textcolor = #ffffff60` (muted white), `size=size.tiny`
- Remove the colored box background — too noisy

---

## 📊 NWE Band Visual Upgrade

Current: two plain colored lines
Proposed:
- Fill between upper and lower with `color.new(#00e5cc, 95)` (nearly invisible teal tint)
- Upper band: `#00e5cc` with 3-layer glow simulation
- Lower band: `#ff6b6b` with 3-layer glow simulation
- When price is ABOVE upper band: band fill turns `#ff313120` (danger red tint)
- When price is BELOW lower band: band fill turns `#39ff1420` (opportunity green tint)

---

## 🔲 OI Delta Visual Upgrade (Sub-Pane)

Remove the `plotcandle()` OI representation — candles for OI on a price chart is confusing and renders at wrong scale.

Replace with:
- **Histogram in sub-pane** (same sub-pane as CVD)
- Bar color: `newLongs=cyan`, `newShorts=magenta`, `shortsClosing=purple`, `longsClosing=gold`, `chop=slate`
- Zero baseline prominent
- Add a 10-bar EMA overlay on the histogram to show OI trend direction

---

## 🚦 Priority Order for Visual Work

1. Define and centralize color constants at top of script (replace all inline color literals)
2. Move OI Delta to sub-pane as histogram
3. Move CVD divergence circles to CVD sub-pane
4. Implement 3-layer glow on NWE bands
5. Design and implement the composite BUY/SELL ZONE label
6. Build the new dashboard table with live regime/score readout
7. Redesign Z-Score sub-pane with gradient background zones
8. Style HH/HL/LH/LL labels (remove colored boxes)
9. Remove all dead plotshape() duplicates
10. Final pass: consistency check all colors against palette table above