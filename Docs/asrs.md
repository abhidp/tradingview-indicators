I am building a TradingView Pine script that marks out the 4th 5-minute candle after the DAX open, and I need to ensure that the code is consistent with the ASRS Strategy Checklist. The code should include logic for breakout trades based on the high and low of the 4th candle, with appropriate stop loss and entry conditions.

Please create a Pine script that implements the ASRS strategy as described in the checklist, ensuring it adheres to the rules and guidelines provided. The script should be clear, concise, and ready for use in TradingView.

It should draw a green line for the buy stop level, a red line for the sell stop level, and a blue line for the stop loss level. The script should also include comments explaining each part of the code.
The line should not extend beyond 1 hour after the 4th candle closes

# ASRS Strategy Checklist

---

## 🔍 **Strategy Summary: Advanced School Run Strategy (ASRS)**

**Instrument**: German DAX Index
**Timeframe**: 5-minute chart
**Focus**: The **4th 5-minute candle** after the official DAX cash open (9:00 AM Frankfurt time)

### 🔑 Core Concept

- **Trade breakouts** from the 4th 5-minute candle’s range.
- **Buy** if price breaks above the 4th bar high + buffer.
- **Sell short** if price breaks below the 4th bar low – buffer.
- **Stop loss** is always placed at the opposite breakout level (tight risk).
- **No fixed profit target**; manage trades manually.

---

## ✅ **Trade Entry Checklist for ASRS**

### 🔁 **PRE-MARKET PREP (Before DAX open)**

- [ ] Confirm local time equivalent of **Frankfurt 9:00 AM**.
- [ ] Open the **5-minute chart** for the DAX index.
- [ ] Mark the **first 4 candles** after the open:

  - Candle 1: 9:00–9:05
  - Candle 2: 9:05–9:10
  - Candle 3: 9:10–9:15
  - **Candle 4**: 9:15–9:20 → This is your **signal candle**.

---

### 📊 **TRADE SETUP (After 4th candle closes)**

- [ ] Note the **high** and **low** of the 4th candle.
- [ ] Calculate entry levels:

  - **Buy stop**: High of 4th bar **+ 2 points**
  - **Sell stop**: Low of 4th bar **– 2 points**

- [ ] Set both pending orders.
- [ ] **Stop loss** = Opposite breakout level

  - If long, SL = short entry level
  - If short, SL = long entry level

- [ ] **No target**: manage trade manually, e.g., trailing stop, partial exits, breakeven move, etc.

---

### ⚠️ **TRADE MANAGEMENT RULES**

- [ ] **Enter only once per side** per setup (unless you've predefined a re-entry rule).
- [ ] If both long and short get triggered (i.e., whipsaw), manage risk strictly.
- [ ] On **very small 4th candle ranges** (low volatility days), consider waiting for the **5th bar** (9:20–9:25) before placing trades.
- [ ] Avoid trading setups **after 2 hours** of the DAX open—strategy is optimized for early market activity.

---
