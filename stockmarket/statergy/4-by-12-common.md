### 4/12 common

- when close is above 160 SMA consider long only
- when close is below 160 SMA consider short only

```bash
//@version=5
strategy("SMA 4/12", overlay=true)

// Define SMA lengths
shortSMALength = 4
longSMALength = 12
longlongSMALength = 160


isGreen(close, open) => close > open
isRed(close, open) => close < open


// Inputs for colors
shortSMAColor = input.color(color.red, title="Short SMA Color (4 SMA)")
longSMAColor = input.color(color.blue, title="Long SMA Color (12 SMA)")

// Calculate SMAs
shortSMA = ta.sma(close, shortSMALength)
longSMA = ta.sma(close, longSMALength)
longlongSMA = ta.sma(close, longlongSMALength)


// Plot the SMAs with the input colors
plot(shortSMA, color=shortSMAColor, linewidth=2, title="4 SMA")
plot(longSMA, color=longSMAColor, linewidth=2, title="12 SMA")

plot(longlongSMA, color=color.green, linewidth=2, title="160 SMA")

// Define cross under condition
shortCondition = ta.crossunder(shortSMA, longSMA) and close < longlongSMA
longCondition = ta.crossover(shortSMA, longSMA) and close > longlongSMA

// Define stop loss as the highest high for the last 6 bars
var float stopLossPrice = na
var float takeProfitPrice = na
var line stopLossLine = na

if (shortCondition)
    //stopLossPrice := ta.highest(6)  // Highest high of last 6 bars
    a = array.from(high, high[1], high[2], high[3], high[4], high[5])
    stopLossPrice := array.max(a, 0)
    entryPrice = close
    risk = stopLossPrice - entryPrice
    takeProfitPrice := entryPrice - risk  // 1:1 risk-reward
    // Draw new stop loss line

    stopLossLine := line.new(x1=bar_index, y1=stopLossPrice, x2=bar_index+5, y2=stopLossPrice, color=color.red, width=2, style=line.style_solid)


// Execute short trade
if (shortCondition)
    strategy.entry("Short", strategy.short)

// Set stop loss and take profit
if (strategy.position_size < 0)  // Check if short position is active
    strategy.exit("Exit", from_entry="Short", stop=stopLossPrice, limit=takeProfitPrice)

if (longCondition)
    //stopLossPrice := ta.highest(6)  // Highest high of last 6 bars
    a = array.from(low, low[1], low[2], low[3], low[4], low[5])
    stopLossPrice := array.min(a, 0)
    entryPrice = close
    risk = entryPrice - stopLossPrice
    takeProfitPrice := entryPrice + risk  // 1:1 risk-reward
    // Draw new stop loss line
    stopLossLine := line.new(x1=bar_index, y1=stopLossPrice, x2=bar_index+5, y2=stopLossPrice, color=color.red, width=2, style=line.style_solid)


// Execute long trade
if (longCondition)
    strategy.entry("long", strategy.long)

// Set stop loss and take profit
if (strategy.position_size > 0)  // Check if long position is active
    strategy.exit("Exit", from_entry="long", stop=stopLossPrice, limit=takeProfitPrice)

threeGreenCandles = isGreen(close[0], open[0]) and isGreen(close[1], open[1]) and isGreen(close[2], open[2])
alertcondition(threeGreenCandles, "3 Green Candle" , message="3 Green Candles")

threeRedCandles = isRed(close[0], open[0]) and isRed(close[1], open[1]) and isRed(close[2], open[2])
alertcondition(threeRedCandles, "3 Red Candle" ,message="3 Red Candles")

twoGreenCandles = isGreen(close[0], open[0]) and isGreen(close[1], open[1])
alertcondition(twoGreenCandles, "2 Green Candle" , message="2 Green Candles")

twoRedCandles = isRed(close[0], open[0]) and isRed(close[1], open[1])
alertcondition(twoRedCandles, "2 Red Candle" ,message="2 Red Candles")
```
