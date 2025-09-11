Here’s a **well-documented version** of your final Pine Script code.
I've added detailed comments explaining each section, its purpose, and how the logic works.

---

```pinescript
// ==============================================================================
// TradingView Pine Script™ Indicator
// ==============================================================================
/*
    Name: Simple Moving Average with Buy/Sell Alerts
    Version: 1.0
    Author: efficientChile41178
    License: Mozilla Public License 2.0 (https://mozilla.org/MPL/2.0/)

    DESCRIPTION:
    This script is designed to identify strong trending and reversal setups using
    multiple SMA crossovers, ADX strength filtering, range consolidation detection,
    and pattern-based breakout/breakdown logic.

    MAIN FEATURES:
    1. Plots:
       - SMA44 with dynamic color (trend strength visualization)
       - SMA44 ± 0.50% Upper & Lower Bands
       - Range threshold lines based on SMA44 ± Range Percent

    2. Buy Conditions:
       - SMA20 crossover with strong ADX and SMA44 rising
       - Range breakout above SMA44
       - SMA44 crossover with strong ADX average

    3. Sell Conditions (Reverse Logic):
       - SMA20 crossunder with strong ADX and SMA44 falling
       - Range breakdown below SMA44
       - SMA44 crossunder with strong ADX average

    4. Consolidation Detection:
       - Detects when price stays in a tight range for 10 candles
       - Alerts when breakout or breakdown occurs

    5. Candlestick Pattern Logic:
       - Detects specific reversal candle patterns such as:
         a) Red → Green → Green for bullish reversals
         b) Green → Red → Red for bearish reversals

    ALERTS PROVIDED:
    - Final Buy Signal
    - Final Sell Signal
    - Consolidation Breakout
    - Consolidation Breakdown
    - Sideways Pattern Buy/Sell

*/

// ==============================================================================
// INPUTS & BASIC SETTINGS
// ==============================================================================
indicator('Simple Moving AVG', overlay = true)

// Inputs for SMA44 and Range Settings
length = input.int(44, minval = 1, title = 'Length (SMA44)', group = 'General Settings')
band_percent = input.float(0.50, "SMA44 Band %", step=0.01, tooltip="Percentage above and below SMA44")
range_percent = input.float(0.1, title="Range Percent", step=0.01, tooltip="Percentage for consolidation range detection")

// Calculate SMA44 and SMA8
ma = ta.sma(close, length)
eightSMA = ta.sma(close, 8)

// Helper price flags
priceBelowSMA = close < ma
priceAboveSMA = close > ma

// Upper and lower bands for SMA44 ± 0.50%
upper_band = ma * (1 + band_percent/100)
lower_band = ma * (1 - band_percent/100)

// Range threshold levels for consolidation detection
rangeUpperThreshold = ma * (1 + range_percent / 100)
rangeLowerThreshold = ma * (1 - range_percent / 100)

// ==============================================================================
// PLOT CONFIGURATION
// ==============================================================================

// Dynamic SMA44 color to show trend strength
d = ta.change(ma)
g = ta.change(d)
ad = ta.cum(math.abs(d)) / bar_index
ag = ta.cum(math.abs(g)) / bar_index

while ad < 1
    ad := ad * 10
    d := d * 10
    d

while ag < 1
    ag := ag * 10
    g := g * 10
    g

ck = 1
fd(x, y) =>
    100 - 200 / (math.pow(1 + ck / y, x) + 1)

delta = fd(d, ad)
f = 2.55 * delta / 2 + 50 * 2.55
lineColor = color.rgb(math.min(510 - 2 * f, 255), math.min(2 * f, 255), 0)

// Plot main SMA44 and bands
plot(ma, color = lineColor, linewidth = 2, title="SMA44")
plot(upper_band, "Upper Band (SMA44 +0.50%)", color=color.black, style=plot.style_line)
plot(lower_band, "Lower Band (SMA44 -0.50%)", color=color.black, style=plot.style_line)
plot(rangeUpperThreshold, "Upper Range Threshold", color=color.black, style=plot.style_line)
plot(rangeLowerThreshold, "Lower Range Threshold", color=color.black, style=plot.style_line)

// ==============================================================================
// TREND FILTERS & DMI (ADX)
// ==============================================================================
[plusDI, minusDI, adx] = ta.dmi(14,14)
adxFilter = adx >= 20
adxSideWays = adx < 20

// ==============================================================================
// SMA CALCULATIONS
// ==============================================================================
sma44 = ta.sma(close, 44)
sma20 = ta.sma(close, 20)

sma44_rising = sma44 > sma44[1]
sma44_falling = sma44 < sma44[1]
sma20_rising = sma20 > sma20[1]
sma20_falling = sma20 < sma20[1]

// Threshold lines for SMA44 ± 0.50%
upperThreshold = sma44 * (1 + band_percent / 100)
lowerThreshold = sma44 * (1 - band_percent / 100)

// ==============================================================================
// SIDEWAYS CANDLE PATTERN CONDITIONS
// ==============================================================================
/*
    BUY PATTERN:
    - 3 consecutive candles: Red → Green → Green
    - Current green candle closes above red candle high
    - Any of the last three candle lows is below lower SMA44 band
*/
redCandle_2   = close[2] < open[2]  
greenCandle_1 = close[1] > open[1]
greenCandle_0 = close > open
patternR_G_G = redCandle_2 and greenCandle_1 and greenCandle_0
closeBelowRedLow = close > high[2]
lowBelowLowerThreshold = (low < lowerThreshold) or (low[1] < lowerThreshold[1]) or (low[2] < lowerThreshold[2])
buyConditionSideways = patternR_G_G and closeBelowRedLow and lowBelowLowerThreshold

/*
    SELL PATTERN:
    - 3 consecutive candles: Green → Red → Red
    - Current red candle closes below green candle low
    - Any of the last three candle highs is above upper SMA44 band
*/
greenCandle_2 = close[2] > open[2]
redCandle_1   = close[1] < open[1]
redCandle_0   = close < open
patternG_R_R = greenCandle_2 and redCandle_1 and redCandle_0
closeAboveGreenHigh = close < low[2]
highAboveUpperThreshold = (high > upperThreshold) or (high[1] > upperThreshold[1]) or (high[2] > upperThreshold[2])
sellConditionSideways = patternG_R_R and closeAboveGreenHigh and highAboveUpperThreshold

// Alerts for sideways patterns
alertcondition(buyConditionSideways, title="BUY ALERT SIDEWAYS", message="Red → Green → Green Pattern Detected (Bullish)")
alertcondition(sellConditionSideways, title="SELL ALERT SIDEWAYS", message="Green → Red → Red Pattern Detected (Bearish)")

// ==============================================================================
// CONSOLIDATION DETECTION
// ==============================================================================
/*
    Logic:
    - Detect when the last 10 candles' OHLC stay within a defined range
    - If breakout above or breakdown below occurs, generate alert
*/
var int lookbackBars = 10
inRange = true
for i = 1 to lookbackBars
    inRange := inRange and (
        (high[i] > rangeUpperThreshold[i] and low[i] < rangeUpperThreshold[i] and low[i] > rangeLowerThreshold[i]) or
        (close[i] < rangeUpperThreshold[i] and close[i] > rangeLowerThreshold[i]) or
        (open[i] < rangeUpperThreshold[i] and open[i] > rangeLowerThreshold[i]) or
        (low[i] < rangeLowerThreshold[i] and high[i] > rangeLowerThreshold[i] and high[i] < rangeUpperThreshold[i])
    )

consolidation_condition = inRange

// Breakout and breakdown definitions
breakout_condition = ta.crossover(close, rangeUpperThreshold)
breakdown_condition = ta.crossunder(close, rangeLowerThreshold)

// ==============================================================================
// BUY CONDITIONS
// ==============================================================================

// ---- 20 SMA Crossover
last3_lows_above_sma44 = low[1] > sma44[1] and low[2] > sma44[2] and low[3] > sma44[3]
buy_on_20_crossover = ta.crossover(close, sma20) and last3_lows_above_sma44 and (adx > 20) and sma44_rising

// ---- Range Breakout Crossover
last5_highs_above_sma44 = high[1] > sma44[1] and high[2] > sma44[2] and high[3] > sma44[3] and high[4] > sma44[4] and high[5] > sma44[5]
buy_on_range_crossover = breakout_condition and (adx > 23) and sma44_rising and last5_highs_above_sma44

// ---- 44 SMA Crossover
avgADX14 = ta.sma(adx, 14)
sma_44_crossover = ta.crossover(close, sma44)
buy_on_44_crossover = (avgADX14 > 20) and sma_44_crossover and sma44_rising

// Combine all buy conditions
final_buy_condition = buy_on_20_crossover or buy_on_range_crossover or buy_on_44_crossover

// ==============================================================================
// SELL CONDITIONS (Reverse Logic of Buy)
// ==============================================================================
last3_highs_below_sma44 = high[1] < sma44[1] and high[2] < sma44[2] and high[3] < sma44[3]
sell_on_20_crossunder = ta.crossunder(close, sma20) and last3_highs_below_sma44 and (adx > 20) and sma44_falling

last5_lows_below_sma44 = low[1] < sma44[1] and low[2] < sma44[2] and low[3] < sma44[3] and low[4] < sma44[4] and low[5] < sma44[5]
sell_on_range_crossunder = breakdown_condition and (adx > 23) and sma44_falling and last5_lows_below_sma44

sma_44_crossunder = ta.crossunder(close, sma44)
sell_on_44_crossunder = (avgADX14 > 20) and sma_44_crossunder and sma44_falling

// Combine all sell conditions
final_sell_condition = sell_on_20_crossunder or sell_on_range_crossunder or sell_on_44_crossunder

// ==============================================================================
// FINAL ALERT COMBINATIONS
// ==============================================================================
finalConsolidationBreakOut = consolidation_condition and breakout_condition
finalConsolidationBreakDown = consolidation_condition and breakdown_condition

// ==============================================================================
// ALERT CONDITIONS
// ==============================================================================

// Buy alerts
alertcondition(final_buy_condition,
     title="Buy Signal Triggered",
     message="Triggered by SMA20 Crossover, Range Breakout, or SMA44 Crossover")

// Sell alerts
alertcondition(final_sell_condition,
     title="Sell Signal Triggered",
     message="Triggered by SMA20 Crossunder, Range Breakdown, or SMA44 Crossunder")

// Consolidation breakout
alertcondition(finalConsolidationBreakOut,
     title="Final Consolidation Breakout",
     message="Consolidation detected and breakout occurred")

// Consolidation breakdown
alertcondition(finalConsolidationBreakDown,
     title="Final Consolidation Breakdown",
     message="Consolidation detected and breakdown occurred")
```

---

### **Summary of Key Logic**

| **Condition**                | **Buy Logic**                                                                                | **Sell Logic**                                                                                |
| ---------------------------- | -------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| **20 SMA Cross**             | `crossover(close, sma20)` + last 3 lows above SMA44 + ADX > 20 + SMA44 rising                | `crossunder(close, sma20)` + last 3 highs below SMA44 + ADX > 20 + SMA44 falling              |
| **Range Breakout/Breakdown** | `crossover(close, rangeUpperThreshold)` + last 5 highs above SMA44 + ADX > 23 + SMA44 rising | `crossunder(close, rangeLowerThreshold)` + last 5 lows below SMA44 + ADX > 23 + SMA44 falling |
| **44 SMA Cross**             | `crossover(close, sma44)` + SMA44 rising + avg ADX(14) > 20                                  | `crossunder(close, sma44)` + SMA44 falling + avg ADX(14) > 20                                 |

---

### **Generated Alerts**

1. **Buy Alert** – Triggered by any of the three buy conditions.
2. **Sell Alert** – Triggered by any of the three sell conditions.
3. **Consolidation Breakout** – Price exits consolidation range upwards.
4. **Consolidation Breakdown** – Price exits consolidation range downwards.
5. **Sideways Pattern Alerts** – Special candle patterns for reversals.

---

This documentation should help you **maintain, extend, or debug** the Pine Script effectively while providing a clear understanding of the strategy logic.
