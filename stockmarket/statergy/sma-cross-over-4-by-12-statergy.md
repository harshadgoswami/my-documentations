### WHEN sma 4 cross over sma 12 on that time take position

```bash
//@version=5
strategy("SMA Cross Over Long with R:R 1:1", overlay=true)

// Define SMA lengths
shortSMALength = 4
longSMALength = 12
longlongSMALength = 160

// Inputs for colors
shortSMAColor = input.color(color.rgb(116, 125, 124), title="long SMA Color (4 SMA)")
longSMAColor = input.color(color.rgb(255, 255, 255), title="Long SMA Color (12 SMA)")

// Calculate SMAs
shortSMA = ta.sma(close, shortSMALength)
longSMA = ta.sma(close, longSMALength)
longlongSMA = ta.sma(close, longlongSMALength)


// Plot the SMAs with the input colors
plot(shortSMA, color=shortSMAColor, linewidth=2, title="4 SMA")
plot(longSMA, color=longSMAColor, linewidth=2, title="12 SMA")

plot(longlongSMA, color=color.green, linewidth=2, title="160 SMA")

// Define cross under condition
longCondition = ta.crossover(shortSMA, longSMA)

// Define stop loss as the highest high for the last 6 bars
var float stopLossPrice = na
var float takeProfitPrice = na
var line stopLossLine = na

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

// Extend the stop loss line while the trade is running
//if (strategy.position_size < 0)
///    line.set_x2(stopLossLine, bar_index)  // Extend the line to the current bar
//else
//    line.delete(stopLossLine)  // Delete the line when the trade is closed
```
