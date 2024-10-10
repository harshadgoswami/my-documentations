### SMA FALLING SORT

## condition as following

1. last 5 sma falling
2. last candle green
3. current candle red
4. current cnadle close less then low of last candle

```bash
//@version=5
strategy("SMA FALLING SHORT 2", overlay=true)

// Calculate SMAs
ma = ta.sma(close, 8)
sma160 = ta.sma(close, 160)

// Determine if the current candle is red

isGreen(close, open) => close > open
isRed(close, open) => close < open

// Function to identify engulfing bearish pattern
isEngulfingBearish() =>
close < open and close[1] > open[1] and high > close[1] and close < low[1]

isEngulfingBullish() =>
close > open and low < close[1] and close > high[1]

// condition
// last 5 sma falling
// last candle green
// current candle red
// current cnadle close less then low of last candle

entryCondition = isRed(close, open) and isGreen(close[1],open[1]) and ma[4] > ma[3] and ma[3] > ma[2] and ma[2] > ma[1] and ma[1] > ma and close < low[1]

entryIndex = bar_index - strategy.opentrades.entry_bar_index(0) + 1

stoploss = high[entryIndex]

profit = stoploss - close[entryIndex]

//var line1 = line.new(0, low, bar_index, high, extend=extend.right)

if(entryIndex < 1)
stoploss := 0

// Exit conditions
exitCondition = close > stoploss or ( entryIndex > 0 and (close[entryIndex] - close ) >= (profit \* 0.7))

// Enter the short trade if entry conditions are met
if strategy.position_size == 0 and entryCondition
strategy.entry("short", strategy.short)

// Exit the short trade if exit conditions are met
if strategy.position_size < 0 and exitCondition
strategy.close("short")
```
