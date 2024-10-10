//@version=5
strategy("8 SMA Long", overlay=true)

// Calculate SMAs
sma = ta.sma(close, 8)

// Plot the SMAs with the input colors

plot(sma , color = color.white)

// Define cross under condition
shortCondition = close > sma and close[1] > sma[1] and close[2] > sma[2] and low < sma and close > open and sma > sma[1] and sma[1] > sma[2]

// Define stop loss as the highest high for the last 6 bars
var float stopLossPrice = na
var float takeProfitPrice = na
var line stopLossLine = na

if (shortCondition)
stopLossPrice := low
entryPrice = close
risk = entryPrice - stopLossPrice
takeProfitPrice := entryPrice + (risk \* 3) // 1:1 risk-reward
// Draw new stop loss line

    stopLossLine := line.new(x1=bar_index, y1=stopLossPrice, x2=bar_index+5, y2=stopLossPrice, color=color.red, width=2, style=line.style_solid)

// Execute short trade
if (shortCondition)
strategy.entry("LONG", strategy.long)

// Set stop loss and take profit
if (strategy.position_size > 0) // Check if short position is active
strategy.exit("Exit", from_entry="LONG", stop=stopLossPrice, limit=takeProfitPrice)

//isCrossUnder = ta.crossunder(close, sma)
//if(isCrossUnder)
// strategy.close("LONG")
