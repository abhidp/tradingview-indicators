# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This repository contains custom TradingView indicators written in Pine Script v6. The indicators are designed for technical analysis and trading strategy implementation.

## File Structure

```
indicators/           # Pine Script indicator files (.pine)
Docs/                # Documentation for each indicator
ReadMe.md           # Repository overview
```

## Pine Script Development

### Language and Version

All indicators use **Pine Script version 6** (`//@version=6`). This is critical - do not use syntax from earlier versions.

### Working with Pine Files

Pine Script files have `.pine` extension and are uploaded to TradingView through their web-based Pine Editor. This is not a compilable codebase - there are no build commands, test runners, or package managers.

**To test/use an indicator:**
1. Copy the `.pine` file content
2. Open TradingView Pine Editor in browser
3. Paste and save the script
4. Add to a chart for testing

### Indicator Architecture

Each indicator follows this structure:

1. **Version declaration** - Always `//@version=6` at the top
2. **Indicator declaration** - `indicator(title, shorttitle, overlay, max_labels_count)`
3. **Input parameters** - User-configurable settings via `input.*()` functions
4. **Calculations** - Core logic for signal detection
5. **Plotting** - Visual elements (lines, labels, shapes, tables)
6. **Alerts** - `alertcondition()` for TradingView notifications

### Key Pine Script Patterns

**State Variables:**
```pine
var float my_var = na  // Initialized once, persists across bars
```

**Session Management:**
```pine
is_in_session = not na(time(timeframe.period, session_string, "Europe/Berlin"))
is_new_session = is_in_session and not is_in_session[1]
```

**Drawing Objects:**
- Lines, labels, and tables persist until explicitly deleted
- Use `line.delete()`, `label.delete()` to clean up
- Arrays store multiple drawing objects for batch management

**Bar Indexing:**
- `bar_index` - current bar position
- `[1]`, `[2]` - access previous bars (e.g., `close[1]` = previous close)

## Current Indicators

### 1. ASRS (Advanced School Run Strategy)
**File:** `indicators/asrs-strategy.pine`
**Purpose:** Implements Tom Hougaard's ASRS day trading strategy for DAX
**Key Features:**
- Identifies the 4th 5-minute candle after DAX open (9:00 CET)
- Draws breakout levels with configurable buffer (default 2 points)
- Session-based logic with automatic daily reset
- Candle numbering labels (1-5)
- Time-based line drawing using `xloc.bar_time`

### 2. Top Right Watermark
**File:** `indicators/top-right-watermark.pine`
**Purpose:** Customizable watermark for chart branding
**Key Features:**
- Table-based positioning (9 positions: top/middle/bottom × left/center/right)
- Multi-line display (up to 3 configurable lines)
- Timeframe format conversion (TradingView ↔ MetaTrader style)
- Supports ticker, exchange, custom text
- Uses `table.new()` and `table.cell_set_*()` functions

### 3. VSA Smart Signals
**File:** `indicators/vsa-smart-signals.pine`
**Purpose:** Professional Volume Spread Analysis implementing Wyckoff methodology with intelligent signal scoring
**Key Features:**
- Signal strength scoring system (1-5 stars)
- ATR-based volatility normalization for adaptive thresholds
- Effort vs result analysis (Wyckoff principle)
- Multi-bar pattern detection (climactic action → absorption)
- Support/resistance context awareness
- Trend filter using MA (SMA/EMA/WMA) - **FIXED: was backwards in original**
- Confirmation mode with proper follow-through logic
- Enhanced info table (10 metrics including volatility, S/R, climax detection)
- Dynamic alerts with current values (strength, volume, wick, price)
- All advanced features are optional and disabled by default

## Development Guidelines

### When Modifying Indicators

1. **Preserve user inputs** - Changing input parameters breaks saved user settings
2. **Maintain visual consistency** - Keep existing color schemes and label styles unless requested
3. **Session handling** - Always clean up drawing objects at session start to prevent memory leaks
4. **Performance** - Avoid heavy calculations in every bar; use `var` for state that persists
5. **Timezone awareness** - Use proper timezone strings (e.g., "Europe/Berlin" for DAX)

### Common Tasks

**Adding a new input parameter:**
```pine
new_param = input.float(default_value, "Display Name", group="Group Name", tooltip="Help text")
```

**Drawing a horizontal line:**
```pine
var line my_line = na
if condition
    line.delete(my_line)
    my_line := line.new(x1, y1, x2, y2, xloc=xloc.bar_time, color=my_color, width=2)
```

**Creating a label:**
```pine
label.new(bar_index, price_level, "Text", style=label.style_label_left, color=bg_color, textcolor=text_color)
```

**Adding alerts:**
```pine
alertcondition(signal_condition, title="Alert Name", message="Alert message sent to user")
```

### Documentation Standard

Each indicator has a corresponding markdown file in `Docs/` explaining:
- Purpose and use case
- Key features
- Installation instructions (copy/paste to TradingView)
- Settings overview

When adding new indicators, create matching documentation in `Docs/`.

## Trading Context

These indicators are designed for:
- **DAX trading** (ASRS) - German stock index, 9:00 CET open
- **Volume Spread Analysis** (VSA) - Wyckoff methodology for institutional order flow
- **Chart management** (Watermark) - Professional chart presentation

Understanding the trading methodology behind each indicator is important for making meaningful improvements.
