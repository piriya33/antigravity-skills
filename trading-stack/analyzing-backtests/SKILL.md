---
name: analyzing-backtests
description: Analyze TradingView strategy backtest results to identify edge quality, overfitting risks, and improvement opportunities.
---

# Backtest Analysis Skill

## Triggering Contexts

- User shares TradingView strategy tester results (pasted or file)
- User asks "Is this strategy good?" or "Should I trade this?"
- User wants to compare strategy versions
- After implementing changes from `coding-pinescript`

## Input Handling

### Accept Any Format

Users may provide backtest data as:

1. **Raw paste** from TradingView Performance Summary (messy but parseable)
2. **Excel export** from TradingView (complete multi-sheet data)
3. **CSV trade list** (individual trade data)
4. **Screenshot** (extract visually)

### Semantic Parsing Rules

Do NOT rely on exact formatting. Look for these concepts flexibly:

| Metric | Variations to Recognize |
|--------|------------------------|
| Net Profit | "Net P&L", "Net P/L", "Total Profit", "Net profit" |
| Total Trades | "Total trades", "# Trades", "Trade count" |
| Win Rate | "Percent profitable", "% Profitable", "Win %", "Winning %" |
| Profit Factor | "PF", "Profit factor" |
| Max Drawdown | "Max DD", "Maximum drawdown", "Max equity drawdown" |
| Avg Win | "Avg winning trade", "Average win" |
| Avg Loss | "Avg losing trade", "Average loss" |
| Sharpe Ratio | "Sharpe", "Sharpe ratio" |
| Sortino Ratio | "Sortino", "Sortino ratio" |

### Value Extraction

After finding a metric name, extract:

- Number with `$` or `USD` → Dollar value
- Number with `%` → Percentage
- Plain number → Raw value

### Fallback

If parsing fails, ask user to confirm key metrics directly.

---

## Core Analysis Framework

### Step 1: Extract & Validate Metrics

**Must Have (required for analysis):**

- Net Profit ($ and %)
- Total Trades
- Win Rate (%)
- Profit Factor
- Max Drawdown ($ and %)
- Avg Win / Avg Loss

**Nice to Have:**

- Sharpe Ratio
- Sortino Ratio
- CAGR
- Avg Bars in Trade

### Step 2: Sanity Checks

Flag if any of these are true:

- [ ] Profit Factor < 0 or > 10 → Parsing error or suspicious
- [ ] Win Rate > 100% or < 0% → Parsing error
- [ ] Total Trades < 30 → Statistically insignificant
- [ ] Max DD > Net Profit → Strategy may not be viable

### Step 3: Quality Assessment

#### Statistical Significance

| Trades | Assessment |
|--------|------------|
| < 30 | ❌ Insufficient - results meaningless |
| 30-100 | ⚠️ Marginal - proceed with caution |
| 100-500 | ✅ Acceptable - reasonable confidence |
| > 500 | ✅ Strong - high confidence |

#### Profit Factor Interpretation

| PF | Assessment |
|----|------------|
| < 1.0 | ❌ Losing strategy |
| 1.0 - 1.2 | ⚠️ Weak edge - transaction costs may kill it |
| 1.2 - 1.5 | ✅ Decent edge - tradeable with discipline |
| 1.5 - 2.0 | ✅ Good edge |
| > 2.0 | ⚠️ Excellent OR overfitted - verify out-of-sample |

#### Drawdown Analysis

| Max DD vs Net Profit | Assessment |
|---------------------|------------|
| DD < 50% of Profit | ✅ Healthy risk/reward |
| DD = 50-100% of Profit | ⚠️ Moderate risk |
| DD > Net Profit | ❌ Risk exceeds reward |

#### Expectancy Check

```
Expectancy = (Win% × Avg Win) - (Loss% × Avg Loss)
```

Must be positive. Calculate per-trade expected value.

### Step 4: Red Flag Detection

**Overfitting Indicators:**

- [ ] Very high PF (> 3.0) with low trade count (< 100)
- [ ] Win rate extremely high (> 80%) for trend strategy
- [ ] Tested on single asset only
- [ ] No out-of-sample validation mentioned

**Survivorship Bias:**

- [ ] Only tested on assets that "worked"
- [ ] No mention of failed variations

**Curve Fitting:**

- [ ] Many optimized parameters
- [ ] Strategy logic too complex for trade count

### Step 5: Probing Questions

Always ask:

1. "Was this tested on out-of-sample data (walk-forward or separate period)?"
2. "How many parameters were optimized?"
3. "Does this logic make sense fundamentally, or is it just pattern-matched?"

---

## Output Format

### Summary Card

```
📊 BACKTEST ANALYSIS: [Strategy Name]

Edge Quality: [STRONG/MODERATE/WEAK/NONE]
Trade Count: [N] trades → [Significance assessment]
Profit Factor: [X] → [Interpretation]
Max Drawdown: [X%] → [Risk assessment]
Expectancy: $[X] per trade

⚠️ WARNINGS:
- [List any red flags]

✅ STRENGTHS:
- [List positive aspects]

📋 RECOMMENDATIONS:
1. [Actionable next steps]
```

### Verdict Categories

| Verdict | Meaning |
|---------|---------|
| ✅ TRADEABLE | Passes all checks, reasonable edge |
| ⚠️ NEEDS WORK | Has potential but issues to fix |
| ❌ NOT READY | Fundamental problems, don't trade |
| 🔍 MORE DATA NEEDED | Can't assess without additional info |

---

## Cross-References

- After analysis, if improvements needed → `coding-pinescript`
- If strategy needs redesign → `planning-trading-systems`
- Track changes between versions → `iterating-strategies`
