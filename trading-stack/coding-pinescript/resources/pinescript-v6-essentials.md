# Pine Script v6 Essential Reference

> Embedded reference for common functions to prevent hallucination.
> For functions not listed here, consult: <https://www.tradingview.com/pine-script-reference/v6/>

---

## Technical Analysis (ta.*)

| Function | Syntax | Returns |
|----------|--------|---------|
| `ta.sma` | `ta.sma(source, length)` | Simple Moving Average |
| `ta.ema` | `ta.ema(source, length)` | Exponential Moving Average |
| `ta.wma` | `ta.wma(source, length)` | Weighted Moving Average |
| `ta.vwma` | `ta.vwma(source, length)` | Volume Weighted MA |
| `ta.rsi` | `ta.rsi(source, length)` | RSI (0-100) |
| `ta.stoch` | `ta.stoch(close, high, low, length)` | Stochastic %K |
| `ta.macd` | `ta.macd(source, fast, slow, signal)` | [macdLine, signalLine, histogram] |
| `ta.atr` | `ta.atr(length)` | Average True Range |
| `ta.tr` | `ta.tr` | True Range (current bar) |
| `ta.bb` | `ta.bb(source, length, mult)` | [middle, upper, lower] |
| `ta.kc` | `ta.kc(source, length, mult)` | Keltner Channel [mid, upper, lower] |
| `ta.cci` | `ta.cci(source, length)` | Commodity Channel Index |
| `ta.mfi` | `ta.mfi(source, length)` | Money Flow Index |
| `ta.obv` | `ta.obv` | On Balance Volume |
| `ta.adx` | `ta.adx(diLen, adxLen)` | Average Directional Index |

## Signal Detection

| Function | Syntax | Returns |
|----------|--------|---------|
| `ta.crossover` | `ta.crossover(a, b)` | true when a crosses above b |
| `ta.crossunder` | `ta.crossunder(a, b)` | true when a crosses below b |
| `ta.cross` | `ta.cross(a, b)` | true on any cross |
| `ta.rising` | `ta.rising(source, length)` | true if rising for length bars |
| `ta.falling` | `ta.falling(source, length)` | true if falling for length bars |
| `ta.change` | `ta.change(source, length)` | difference from length bars ago |

## Highest/Lowest

| Function | Syntax | Returns |
|----------|--------|---------|
| `ta.highest` | `ta.highest(source, length)` | Highest value |
| `ta.lowest` | `ta.lowest(source, length)` | Lowest value |
| `ta.highestbars` | `ta.highestbars(source, length)` | Bars since highest |
| `ta.lowestbars` | `ta.lowestbars(source, length)` | Bars since lowest |

---

## Strategy Functions

| Function | Syntax | Description |
|----------|--------|-------------|
| `strategy.entry` | `strategy.entry(id, direction, qty, limit, stop, comment)` | Open position |
| `strategy.exit` | `strategy.exit(id, from_entry, qty, limit, stop, trail_points, trail_offset)` | Set TP/SL/Trailing |
| `strategy.close` | `strategy.close(id, comment)` | Close position by ID |
| `strategy.close_all` | `strategy.close_all(comment)` | Close all positions |
| `strategy.cancel` | `strategy.cancel(id)` | Cancel pending order |
| `strategy.cancel_all` | `strategy.cancel_all()` | Cancel all pending |

**Strategy Variables:**

| Variable | Type | Description |
|----------|------|-------------|
| `strategy.position_size` | float | Current position (+ long, - short, 0 flat) |
| `strategy.position_avg_price` | float | Average entry price |
| `strategy.equity` | float | Current equity |
| `strategy.netprofit` | float | Net profit |
| `strategy.grossprofit` | float | Gross profit |
| `strategy.grossloss` | float | Gross loss |
| `strategy.openprofit` | float | Unrealized P&L |

---

## Request Functions

| Function | Syntax | Notes |
|----------|--------|-------|
| `request.security` | `request.security(symbol, timeframe, expression, gaps, lookahead)` | ⚠️ Beware repainting |
| `request.security_lower_tf` | `request.security_lower_tf(symbol, timeframe, expression)` | Lower TF data |
| `request.financial` | `request.financial(symbol, financial_id, period)` | Fundamental data |
| `request.quandl` | `request.quandl(ticker, gaps)` | Quandl data |

**Safe `request.security` usage:**

```pinescript
// Non-repainting patterns:
value = request.security(syminfo.tickerid, "D", close[1], lookahead=barmerge.lookahead_on)
// OR
value = request.security(syminfo.tickerid, "D", close)  // lookahead_off default
```

---

## Input Functions (v6 Style)

| Function | Syntax |
|----------|--------|
| `input.int` | `input.int(defval, title, minval, maxval, step, tooltip, group)` |
| `input.float` | `input.float(defval, title, minval, maxval, step, tooltip, group)` |
| `input.bool` | `input.bool(defval, title, tooltip, group)` |
| `input.string` | `input.string(defval, title, options, tooltip, group)` |
| `input.source` | `input.source(defval, title, tooltip, group)` |
| `input.timeframe` | `input.timeframe(defval, title, tooltip, group)` |
| `input.color` | `input.color(defval, title, tooltip, group)` |
| `input.price` | `input.price(defval, title, tooltip, group)` |

---

## Built-in Variables

| Variable | Description |
|----------|-------------|
| `open`, `high`, `low`, `close` | OHLC prices |
| `volume` | Volume |
| `time` | Bar timestamp (UNIX ms) |
| `bar_index` | Bar number (0 = oldest) |
| `na` | Not Available (null) |

**Symbol Info:**

| Variable | Description |
|----------|-------------|
| `syminfo.tickerid` | Full symbol ID |
| `syminfo.ticker` | Ticker name |
| `syminfo.mintick` | Minimum tick size |
| `syminfo.pointvalue` | Point value (futures) |

**Bar State:**

| Variable | Description |
|----------|-------------|
| `barstate.islast` | Is last bar |
| `barstate.isrealtime` | Is realtime bar |
| `barstate.isconfirmed` | Is confirmed (closed) |
| `barstate.isnew` | Is new bar |

---

## Type System (Critical!)

> **Most common AI error:** Confusing `series` vs `simple` types, especially when functions expect `simple bool` but receive `series bool`.

### Type Qualifiers

| Qualifier | Meaning | Example |
|-----------|---------|---------|
| `const` | Compile-time constant | `const int x = 5` |
| `input` | User input (fixed after start) | `input.int(14)` |
| `simple` | Known at bar 0, never changes | `simple int len = bar_index == 0 ? 10 : 10` |
| `series` | Can change on every bar | `series float price = close` |

### Type Hierarchy (Implicit Casting)

```
const → input → simple → series
```

A `const` can be used where `series` is expected, but NOT vice versa.

### Common Types

| Type | Qualifiers | Examples |
|------|-----------|----------|
| `int` | const, input, simple, series | `14`, `bar_index` |
| `float` | const, input, simple, series | `1.5`, `close`, `ta.sma(close, 14)` |
| `bool` | const, input, simple, series | `true`, `close > open` |
| `string` | const, input, simple | `"Long"`, `syminfo.ticker` |
| `color` | const, input, series | `color.red`, `close > open ? color.green : color.red` |

### ⚠️ Series vs Simple - The #1 Type Error

**Problem:** Many functions require `simple` types but you pass `series`.

```pinescript
// ❌ ERROR: ta.sma expects simple int for length
int dynamicLen = close > open ? 10 : 20  // This is series int!
float ma = ta.sma(close, dynamicLen)     // ERROR!

// ✅ CORRECT: Use input or const for length
int staticLen = input.int(14, "Length")  // This is input int (simple)
float ma = ta.sma(close, staticLen)      // OK!
```

**Key Rule:** Most indicator length/period parameters require `simple int`, not `series int`.

### Type Casting Functions

| Function | Converts To | Example |
|----------|------------|---------|
| `int()` | int | `int(3.7)` → `3` |
| `float()` | float | `float(5)` → `5.0` |
| `bool()` | bool | `bool(1)` → `true`, `bool(0)` → `false` |
| `str.tostring()` | string | `str.tostring(close)` → `"1234.5"` |
| `color.new()` | color | `color.new(color.red, 50)` → semi-transparent red |

### Checking for na (Not Available)

```pinescript
// ❌ WRONG: Cannot compare directly
if myVar == na  // Compiler error or unexpected behavior

// ✅ CORRECT: Use na() function
if na(myVar)
    // handle missing value

// ✅ CORRECT: Use nz() to replace na with default
float safeValue = nz(myVar, 0.0)  // Returns 0.0 if myVar is na
```

### Common Type Mismatch Scenarios

| Scenario | Error | Fix |
|----------|-------|-----|
| Dynamic length | `Cannot call ta.sma with series int` | Use `input.int()` for length |
| Comparing series to simple | `Cannot compare series float to simple float` | Both sides must be compatible |
| Assigning to wrong type | `Cannot assign series to simple` | Declare variable as `series` |
| Using series in string | `Cannot convert series to string` | Use `str.tostring()` |
| Conditional color | Color must be same type both branches | Ensure both are `series color` |

### Explicit Type Declaration Examples

```pinescript
// Always declare types explicitly to avoid confusion:

// Simple types (fixed values)
simple int periods = 14
simple float multiplier = 1.5
simple bool useLong = true

// Series types (change per bar)
series float currentPrice = close
series bool isUpBar = close > open
series color barColor = isUpBar ? color.green : color.red

// Input types (user configurable)
int rsiLength = input.int(defval=14, title="RSI Length")
float atrMult = input.float(defval=1.5, title="ATR Multiplier")
bool showSignals = input.bool(defval=true, title="Show Signals")

// Persistent state (var keyword)
var int totalTrades = 0
var float maxEquity = 0.0
var bool isInTrade = false
```

### Arrays

```pinescript
// Declare arrays with explicit type:
var array<float> prices = array.new_float()
var array<int> signals = array.new_int(10, 0)  // size 10, fill with 0
var array<bool> conditions = array.new_bool()

// Common operations:
array.push(prices, close)
float lastPrice = array.get(prices, array.size(prices) - 1)
array.clear(prices)
```

---

## Common Errors & Fixes

| Error | Cause | Fix |
|-------|-------|-----|
| `Mismatched input` | Syntax error | Check brackets, commas |
| `Cannot call X with arguments` | Wrong parameter types | Check function signature |
| `Undeclared identifier` | Variable not defined | Define before use |
| `Cannot modify global variable` | Trying to assign in local scope | Use `:=` not `=` for reassignment |
| `Cannot convert series to simple` | Dynamic value where static needed | Use input/const for that parameter |
| `line 0: operator expected` | Invalid expression | Check for missing operators |
| `Incompatible types in ternary` | Different types in `?:` branches | Ensure both branches return same type |
