Below is a **single Pine‑v5 strategy** that now supports **both long *and* short trades**, moves the stop to **breakeven at 1 : 1**, and uses a **30‑bar pull‑away** look‑back (instead of 20).

```pinescript
//@version=5
strategy(
     "20/50 SMA Pull‑Away (Long & Short) – SL→BE",
     overlay = true,
     default_qty_type = strategy.percent_of_equity,
     default_qty_value = 100)

// ───────────────── PARAMETERS
sma20_len  = 20
sma50_len  = 50
rr_target  = 3           // final R:R = 1:3
pull_look  = 30          // pull‑away look‑back (changed to 30)

// ───────────────── INDICATORS
sma20 = ta.sma(close, sma20_len)
sma50 = ta.sma(close, sma50_len)

// ───────────────── HELPER CONDITIONS
crossUp20  = ta.crossover(close, sma20)
crossUp50  = ta.crossover(close, sma50)
crossDn20  = ta.crossunder(close, sma20)
crossDn50  = ta.crossunder(close, sma50)

sma20_up   = sma20 > sma20[1]
sma50_up   = sma50 > sma50[1]
sma20_dn   = sma20 < sma20[1]
sma50_dn   = sma50 < sma50[1]

bull_now   = sma20 > sma50
bear_now   = sma20 < sma50

bull_pull  = sma20[pull_look] < sma50[pull_look]   // 30 bars ago 20 SMA was below 50
bear_pull  = sma20[pull_look] > sma50[pull_look]   // 30 bars ago 20 SMA was above 50

// ───────────────── ENTRY RULES
longCond  = (crossUp20 or crossUp50)  and sma20_up and sma50_up and bull_now  and bull_pull
shortCond = (crossDn20 or crossDn50)  and sma20_dn and sma50_dn and bear_now and bear_pull

// ───────────────── TRADE STATE VARS
var float entryP  = na
var float stopLvl = na
var float takeLvl = na
var float riskVal = na
var bool  movedBE = false

// ───────────────── LONG ENTRY
if longCond
    entryP  := close
    stopLvl := low
    riskVal := entryP - stopLvl
    takeLvl := entryP + rr_target * riskVal
    movedBE := false
    strategy.entry("Long", strategy.long)

// ───────────────── SHORT ENTRY
if shortCond
    entryP  := close
    stopLvl := high
    riskVal := stopLvl - entryP
    takeLvl := entryP - rr_target * riskVal
    movedBE := false
    strategy.entry("Short", strategy.short)

// ───────────────── MOVE SL → BREAKEVEN @ 1:1
//   • long: when price ≥ entry + risk
//   • short: when price ≤ entry − risk
if strategy.position_size > 0 and not movedBE and high >= entryP + riskVal
    stopLvl := entryP
    movedBE := true

if strategy.position_size < 0 and not movedBE and low  <= entryP - riskVal
    stopLvl := entryP
    movedBE := true

// ───────────────── EXITS
if strategy.position_size > 0
    strategy.exit("X‑Long",  from_entry = "Long",  stop = stopLvl, limit = takeLvl)

if strategy.position_size < 0
    strategy.exit("X‑Short", from_entry = "Short", stop = stopLvl, limit = takeLvl)

// ───────────────── PLOTS
plot(sma20, title = "SMA 20", color = color.orange, linewidth = 2)
plot(sma50, title = "SMA 50", color = color.blue,   linewidth = 2)

plot(strategy.position_size != 0 ? stopLvl : na,  title = "Stop‑Loss",  color = color.red,   style = plot.style_linebr)
plot(strategy.position_size != 0 ? takeLvl : na,  title = "Take‑Profit",color = color.green, style = plot.style_linebr)
```

### How it works 📋

| Step              | Long logic                        | Short logic                         |
| ----------------- | --------------------------------- | ----------------------------------- |
| **Trend**         | Both SMAs rising                  | Both SMAs falling                   |
| **Current order** | 20 SMA > 50 SMA                   | 20 SMA < 50 SMA                     |
| **Pull‑away**     | 30 bars ago 20 SMA < 50 SMA       | 30 bars ago 20 SMA > 50 SMA         |
| **Trigger**       | Price crosses **up** 20 or 50 SMA | Price crosses **down** 20 or 50 SMA |
| **Initial SL**    | Candle low                        | Candle high                         |
| **TP**            | 3 × risk above entry              | 3 × risk below entry                |
| **Move SL→BE**    | When price ≥ entry + risk         | When price ≤ entry − risk           |

Add the script to your chart, open the **Strategy Tester**, and explore the results.
Feel free to ask for further tweaks—e.g., trail stops after breakeven, alerts, or position‑size filters!
