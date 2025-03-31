Here's a **Pine Script strategy** based on your specified conditions:

---

### **Entry Conditions:**

1. **50 SMA slope is high** (rising trend).
2. **Price crosses up the lower Bollinger Band** (based on a 14 SMA).
3. **Stop Loss**: Below the **lowest low of the last 3 candles**.
4. **Target**: **1:2 risk-to-reward ratio**.

---

### **Pine Script Code:**

```pinescript
//@version=5
strategy("SMA Slope & Bollinger Band Buy Entry", overlay=true)

// Inputs
smaLength = 50
bbLength = 14
bbMult = 2

// Calculate 50 SMA and its slope
sma50 = ta.sma(close, smaLength)
sma50_slope = sma50 - sma50[1]  // Difference to check slope

// Bollinger Bands (based on 14 SMA)
basis = ta.sma(close, bbLength)
dev = bbMult * ta.stdev(close, bbLength)
lowerBB = basis - dev

// Condition 1: 50 SMA is rising (positive slope)
sma50Rising = sma50_slope > 0

// Condition 2: Price crosses above the lower Bollinger Band
crossAboveLowerBB = ta.crossover(close, lowerBB)

// Combine Entry Condition
longEntry = sma50Rising and crossAboveLowerBB

// Stop Loss: Below the lowest low of the last 3 candles
lowestLow3 = ta.lowest(low, 3)

// Variables for stop loss and target
var float stopLossPrice = na
var float targetPrice = na

// Execute Buy Order
if longEntry
    stopLossPrice := lowestLow3
    risk = close - stopLossPrice
    targetPrice := close + 2 * risk  // 1:2 Risk-Reward
    strategy.entry("Buy", strategy.long)

// Exit Conditions: Stop Loss or Target
if strategy.position_size > 0
    strategy.exit("Take Profit", "Buy", stop=stopLossPrice, limit=targetPrice)

// Plot Indicators
plot(sma50, title="50 SMA", color=color.blue, linewidth=2)
plot(basis, title="Bollinger Basis", color=color.gray)
plot(lowerBB, title="Lower Bollinger Band", color=color.red)

// Plot Stop Loss & Target Levels
plot(stopLossPrice, color=color.red, linewidth=2, title="Stop Loss")
plot(targetPrice, color=color.green, linewidth=2, title="Take Profit")
```

---

### **Explanation:**

✅ **50 SMA Slope Check**: Uses the difference between `sma50` and `sma50[1]` to confirm an uptrend.  
✅ **Lower Bollinger Band Crossover**: Detects when the price crosses above the **lower Bollinger Band (14 SMA-based)**.  
✅ **Stop Loss**: Sets the SL **below the lowest low of the last 3 candles**.  
✅ **Target**: Aims for **2 times the risk (1:2 Risk-Reward)**.

---

### **How It Works:**

-   The strategy **waits for an uptrend** (50 SMA rising).
-   It **buys** when **price crosses up the lower Bollinger Band**.
-   It sets **Stop Loss & Target** automatically.
-   It plots **SMA, Bollinger Bands, Stop Loss, and Target levels**.

This strategy helps **catch bullish reversals** in a rising trend! 🚀 Let me know if you need changes! 😊
