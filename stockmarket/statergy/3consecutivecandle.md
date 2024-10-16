### WHEN 3 consecutive red or green candle found

```bash
//@version=5
strategy("3 Consecutive Candles", overlay=true)

// Function to detect bullish engulfing pattern (long)
bullish_engulfing = (close[1] < open[1]) and (close > open) and (close > high[1]) // and (open < low[1])

// Function to detect bearish engulfing pattern (short)
bearish_engulfing = (close[1] > open[1]) and (close < open) and (close < low[1]) //and (open > high[1])

// Variables to count green and red candles
consecutive_green_candles = 0
consecutive_red_candles = 0
consecutive_green_last_found_index = 0
consecutive_red_last_found_index = 0

// 5 , 4  , 3 ,  2 ,  1 , 0
// g , g  , g ,  r ,  r , g

// Loop through last 20 bars to count green and red candles
for i = 0 to 19
    if close[i] > open[i] and ( consecutive_green_last_found_index == 0 or ( (consecutive_green_last_found_index + 1) - i ) == 0 ) and consecutive_green_candles < 3
        consecutive_green_candles := consecutive_green_candles + 1
        consecutive_green_last_found_index := i
    if close[i] <  open[i] and consecutive_green_last_found_index > 0 and  consecutive_green_candles < 3
        consecutive_green_last_found_index := 0   // reset if not consecutive
        consecutive_green_candles := 0
    if close[i] < open[i] and ( consecutive_red_last_found_index == 0 or ( (consecutive_red_last_found_index + 1) - i ) == 0 ) and consecutive_red_candles < 3
        consecutive_red_candles := consecutive_red_candles + 1
        consecutive_red_last_found_index := i
    if close[i] >  open[i] and consecutive_red_last_found_index > 0 and consecutive_red_candles < 3
        consecutive_red_last_found_index := 0     // reset if not consecutive
        consecutive_red_candles := 0


if consecutive_red_candles >= 3 and consecutive_green_candles >= 3 and consecutive_green_last_found_index < consecutive_red_last_found_index
    consecutive_red_candles := 0

if consecutive_red_candles >= 3 and consecutive_green_candles >= 3 and consecutive_green_last_found_index > consecutive_red_last_found_index
    consecutive_green_candles := 0


// Condition for long entry: Bullish engulfing pattern + 3 consecutive green candles
find_3_consecutive_green = consecutive_green_candles >= 3
longCondition = bullish_engulfing and find_3_consecutive_green

// Condition for short entry: Bearish engulfing pattern + 3 consecutive red candles
find_3_consecutive_red = consecutive_red_candles >= 3
shortCondition = bearish_engulfing and find_3_consecutive_red

// Variables for stop loss and target prices
var float stopLossPrice = na
var float targetPrice = na

// Check if a long position or short position is already active
longPositionActive = (strategy.position_size > 0)
shortPositionActive = (strategy.position_size < 0)

// Long entry logic
if (longCondition and not shortPositionActive)
    // Set stop loss at the low of the current engulfing (green) candle
    stopLossPrice := low

    // Risk calculation (distance between close and stop loss)
    risk = close - stopLossPrice

    // Target price is 3 times the risk
    targetPrice := close + 3 * risk

    // Place long order
    strategy.entry("Long", strategy.long)

// Short entry logic
if (shortCondition  and not longPositionActive)
    // Set stop loss at the high of the current engulfing (red) candle
    stopLossPrice := high

    // Risk calculation (distance between close and stop loss)
    risk = stopLossPrice - close

    // Target price is 3 times the risk
    targetPrice := close - 3 * risk

    // Place short order
    strategy.entry("Short", strategy.short)

// Exit conditions: exit when target or stop loss is hit
if (longPositionActive)  // Long position
    strategy.exit("Take Profit", "Long", stop=stopLossPrice, limit=targetPrice)

if (shortPositionActive)  // Short position
    strategy.exit("Take Profit", "Short", stop=stopLossPrice, limit=targetPrice)


plot(consecutive_red_last_found_index)
plot(consecutive_green_last_found_index)
```
