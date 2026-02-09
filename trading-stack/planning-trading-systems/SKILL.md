---
name: planning-trading-systems
description: Designs complete trading systems before implementation. Guides through edge definition, entry/exit rules, position sizing, and risk management. Use before writing strategy code.
---

# Planning Trading Systems

## Triggering Contexts

- User wants to build a new trading strategy
- User has a vague idea ("I want to trade momentum")
- Before any Pinescript coding begins
- When existing strategy lacks clear rules

## Workflow Checklist

(Copy into `task.md` when starting)

- [ ] Define the Edge (Why does this work?)
- [ ] Entry Rules (Signal, confirmation, filters)
- [ ] Exit Rules (Profit target, stop loss, time-based)
- [ ] Position Sizing (How much per trade?)
- [ ] Position Management (Trailing, scaling)
- [ ] Risk Management (Drawdown limits, correlation)
- [ ] Expected Metrics (Win rate, R:R, expectancy)
- [ ] Generate strategy-spec.md

---

## Instructions

### 1. Edge Definition

**Ask the user:**

- What market inefficiency are you exploiting?
- What is the alpha source? (Trend, mean-reversion, volatility, information)
- What market conditions does this edge work in?

**Reject vague answers.** "I want to use RSI and MACD" is not an edge.

### 2. Entry Rules

Define precise, unambiguous criteria:

| Component | Description |
|-----------|-------------|
| **Primary Signal** | Main trigger (e.g., EMA crossover) |
| **Confirmation** | Secondary filter (e.g., RSI > 50) |
| **Trend Filter** | Higher TF alignment (e.g., above 200 EMA on D) |
| **Timing** | When to execute (close of bar, limit order) |

**Quality check:** Can a machine execute this without interpretation?

### 3. Exit Rules

| Exit Type | Example |
|-----------|---------|
| **Stop Loss** | 1x ATR below entry |
| **Profit Target** | 2x ATR from entry |
| **Trailing Stop** | Trail by highest high - 1.5 ATR |
| **Time Exit** | Close after 10 bars if no TP/SL hit |

**Rule:** Stop loss must be defined BEFORE entry. Risk-first thinking.

### 4. Position Sizing

| Method | Formula |
|--------|---------|
| **Fixed Fractional** | `Size = (Equity × Risk%) / (Entry - SL)` |
| **Fixed Lot** | Same size every trade |
| **Volatility-Based** | Size inversely proportional to ATR |

Recommend 1-2% risk per trade for most traders.

### 5. Position Management

- **Scale In:** Rules for pyramiding (if any)
- **Scale Out:** Partial TP (e.g., 50% at 1R, rest at 2R)
- **Breakeven:** When to move SL to entry

### 6. Risk Management

| Parameter | Recommended |
|-----------|-------------|
| Max Risk Per Trade | 1-2% |
| Max Daily Loss | 3-5% |
| Max Drawdown | 10-20% |
| Max Open Positions | 3-5 |

### 7. Expected Metrics

Set expectations BEFORE backtesting:

| Metric | Target |
|--------|--------|
| Win Rate | 40-55% |
| Avg R:R | 1.5:1+ |
| Max Drawdown | < 25% |
| Expectancy | Positive |

`Expectancy = (WinRate × AvgWin) - (LossRate × AvgLoss)`

---

## Output Artifact

Generate `strategy-spec.md` using template in `resources/strategy-spec-template.md`.

---

## Cross-References

- **Next Step:** When design is complete → use `coding-pinescript` to implement
- **Input:** Can be triggered after `brainstorming-ideas` for vague trading concepts
