20 SMA 44 SMA


```pinescript
// This Pine Script™ code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © efficientChile41178

//@version=6
indicator('Simple Moving AVG', overlay = true)

length = input.int(44, minval = 1, title = 'Length', group = 'general settings for all indicators.')

ma = ta.sma(close, length)
eightSMA = ta.sma(close, 8)

priceBelowSMA = close < ma
priceAboveSMA = close > ma


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

plot(ma, color = color.rgb(math.min(510 - 2 * f, 255), math.min(2 * f, 255), 0), linewidth = 2)

// ADX via DMI (returns +DI, -DI, ADX)
[plusDI, minusDI, adx] = ta.dmi(14,14)
adxFilter = adx >= 23
adxSideWays = adx < 20


// 44 SMA
sma44 = ta.sma(close, 44)
sma44_rising = sma44 > sma44[1]
sma44_falling = sma44 < sma44[1]


// SIDE WAY MARKET STATERGY
//threshold = 0.49
//diffPercent = ((low - sma44) / sma44) * 100
//longCondSideWays  = diffPercent <= -threshold and adxSideWays   // Price is below SMA by more than threshold
//shortCondSideWays = diffPercent >= threshold and adxSideWays

//plot(diffPercent)

// 20 SMA
sma20 = ta.sma(close, 20)
sma20_rising = sma20 > sma20[1]
sma20_falling = sma20 < sma20[1]

// 160 SMA
sma160 = ta.sma(close,160)
sma160_rising = sma160 > sma160[1]
sma160_falling = sma160 < sma160[1]


// -------------------
// BUY CONDITIONS
// -------------------

// Condition 1: Last candle is red, current candle is green, and current close > last red candle high
cond1_buy = close[1] < open[1] and close > open and close > high[1]

// Condition 2: Second last candle is red, last & current candles are green, and current close > second last red candle high
cond2_buy = close[2] < open[2] and close[1] > open[1] and close > open and close > high[2]

// Final Buy Condition
buyCondition = (sma44_rising and sma20_rising) and (sma20 > sma44) and (cond1_buy or cond2_buy) and adxFilter


// -------------------
// SELL CONDITIONS
// -------------------

// Condition 1: Last candle is green, current candle is red, and current close < last green candle low
cond1_sell = close[1] > open[1] and close < open and close < low[1]

// Condition 2: Second last candle is green, last & current candles are red, and current close < second last green candle low
cond2_sell = close[2] > open[2] and close[1] < open[1] and close < open and close < low[2]

// Final Sell Condition
sellCondition = (sma44_falling  and sma20_falling ) and (sma20 < sma44 ) and (cond1_sell or cond2_sell) and adxFilter

crossUp44 = ta.crossover(close, sma44)
crossDown44 = ta.crossunder(close, sma44)

crossCondition = crossUp44 or crossDown44


// -------------------
// ALERTS
// -------------------
alertcondition(buyCondition, title="Buy Alert", message="Buy Signal: 44 SMA rising with bullish reversal pattern.")
alertcondition(sellCondition, title="Sell Alert", message="Sell Signal: 44 SMA falling with bearish reversal pattern.")
alertcondition(crossCondition, title="Cross 44 SMA", message="Price crossed 44 SMA.")

//alertcondition(longCondSideWays, title="Buy Alert In Sideways", message="Buy Alert In Sideways")
//alertcondition(shortCondSideWays, title="Sell Alert In Sideways", message="Sell Alert In Sideways")


```


