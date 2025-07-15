Colour simple moving average 


```pinescript
// This Pine Script™ code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © efficientChile41178

//@version=5
indicator("Simple Moving AVG", overlay = true)

length = input.int(50, minval=1, title="Length", group = "general settings for all indicators.")

ma = ta.sma(close, length)
eightSMA = ta.sma(close, 8)

priceBelowSMA = close < ma
priceAboveSMA = close > ma


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
```
