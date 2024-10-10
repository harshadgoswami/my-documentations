### Color Moving Average

```bash
// This Pine Script™ code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © efficientChile41178

//@version=5
indicator("Simple Moving AVG", overlay = true)

length = input.int(8, minval=1, title="Length", group = "general settings for all indicators.")

ma = ta.sma(close, length)

d = ta.change(ma)
g = ta.change(d)
ad = ta.cum(math.abs(d))/bar_index
ag = ta.cum(math.abs(g))/bar_index
while ad<1
    ad:= ad*10
    d:= d*10
while ag<1
    ag:= ag*10
    g:= g*10


ck = 1
fd(x,y) =>
    100-200/(math.pow(1+ck/y,x)+1)

delta = fd(d,ad)
f = 2.55*delta/2+50*2.55

plot(ma,color=color.rgb(math.min(510-2*f,255),math.min(2*f,255),0), linewidth = 2)


// setting up alert on cross above or below
cross_above = ma > low and ma < close
cross_below = ma < high and ma > close
alertcondition((cross_above or cross_below), "Moving Average Cross" , message="Moving Average Cross")

// Function to determine if a candle is green
isGreen(close, open) => close > open
isRed(close, open) => close < open

isEngulfingBearish() =>
    close < open and close[1] > open[1]  and close < low[1] and ( open - close  ) > (close[1] - open[1])    // and high > close[1]

isEngulfingBullish() =>
    close > open and close[1] < open[1] and close > high[1] and (close - open) > (open[1] - close[1])  // and low < close[1]

// Check if the last 3 candles are green
threeGreenCandles = isGreen(close[0], open[0]) and isGreen(close[1], open[1]) and isGreen(close[2], open[2])
alertcondition(threeGreenCandles, "3 Green Candle" , message="3 Green Candles")

threeRedCandles = isRed(close[0], open[0]) and isRed(close[1], open[1]) and isRed(close[2], open[2])
alertcondition(threeRedCandles, "3 Red Candle" ,message="3 Red Candles")

twoGreenCandles = isGreen(close[0], open[0]) and isGreen(close[1], open[1])
alertcondition(twoGreenCandles, "2 Green Candle" , message="2 Green Candles")

twoRedCandles = isRed(close[0], open[0]) and isRed(close[1], open[1])
alertcondition(twoRedCandles, "2 Red Candle" ,message="2 Red Candles")

cross_below_from_last_two = twoRedCandles and (((ma < high and ma > close) or (ma[1] < high[1] and ma[1] > close[1])) and (ma < ma[1]))
//alertcondition(twoRedCandles and cross_below_from_last_two, "SIGNAL SELL" , message="SIGNAL SELL")

signal_sell = (( isGreen(close[2],open[2]) and isGreen(close[1],open[1]) and isRed(close[0],open[0]) and (close[0] < low[1] or close[0] < low[2] ) ) or ( isGreen(close[3],open[3]) and isGreen(close[2],open[2]) and isRed(close[1],open[1]) and isRed(close[0],open[0]) and ( close[0] < low[2] or close[0] < low[3] ) ))


alertcondition(isEngulfingBearish(), "SIGNAL SELL" , message="SIGNAL SELL")

cross_above_from_last_two = twoGreenCandles and (( (ma > low and ma < close) or (ma[1] > low[1] and ma[1] < close[1]) ) and (ma > ma[1]))
//alertcondition(twoGreenCandles and cross_above_from_last_two, "SIGNAL BUY" , message="SIGNAL BUY")


signal_buy = (( isRed(close[2],open[2]) and isRed(close[1],open[1]) and isGreen(close[0],open[0]) and (close[0] > high[1] or close[0] > high[2] ) ) or ( isRed(close[3],open[3]) and isRed(close[2],open[2]) and isGreen(close[1],open[1]) and isGreen(close[0],open[0]) and ( close[0] > high[2] or close[0] > high[3] ) ))
alertcondition( isEngulfingBullish(), "SIGNAL BUY" , message="SIGNAL BUY")


alertcondition( isEngulfingBullish(), "Engulfing Bullish" , message="Engulfing Bullish")

alertcondition( isEngulfingBearish(), "Engulfing Bearish" , message="Engulfing Bearish")


alertcondition(ma < ma[1] and ma[1] < ma[2],"Downward Slope" , message="Downward Slope")
alertcondition(ma > ma[1] and ma[1] > ma[2] , "Upward Slope", message="Upward Slope")



//alertcondition(cross_below, message="8-SMA Cross Below")

//maq = ta.sma(close, 22)
//plot(maq, color=color.rgb(7, 38, 6, 37),linewidth = 2)

```
