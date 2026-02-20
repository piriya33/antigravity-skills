---
name: coding-pinescript
description: Writes, reviews, and debugs Pine Script v6 strategies and indicators. Includes anti-pattern prevention, performance optimization, and built-in function reference. Use when implementing trading systems in TradingView.
---

# Coding Pine Script

## Triggering Contexts

- Implementing a strategy from `strategy-spec.md`
- Converting v4/v5 scripts to v6
- Debugging script errors or unexpected behavior
- Reviewing Pinescript code for quality
- Optimizing script performance

## Workflow Checklist

(Copy into `task.md` when starting)

- [ ] Review strategy-spec.md (if available)
- [ ] Write code in v6 syntax
- [ ] Run code review checklist
- [ ] Test compilation in TradingView
- [ ] Verify backtest results match expectations

---

## 1. Version Strategy

**Write new code in v6.** Convert older versions before development.

| Version | Marker |
|---------|--------|
| v4 | `@version=4`, uses `study()` |
| v5 | `@version=5`, uses `indicator()` |
| v6 | `@version=6`, current standard |

**Common v5 → v6 changes:**

- `security()` → `request.security()`
- `input()` → `input.int()`, `input.float()`, `input.bool()`

---

## 2. Anti-Patterns & Best Practices

### ⚠️ Repainting Prevention (Critical!)

```pinescript
// ❌ WRONG (Future leak):
dailyHigh = request.security(syminfo.tickerid, "D", high, lookahead=barmerge.lookahead_on)

// ✅ CORRECT (Offset by 1):
dailyHigh = request.security(syminfo.tickerid, "D", high[1], lookahead=barmerge.lookahead_on)
```

### State Preservation

```pinescript
// ❌ Resets every bar:
count = 0

// ✅ Persists across bars:
var int count = 0
```

### Performance

```pinescript
// ❌ Recalculates every bar:
threshold = 50 * input.int(defval=2, title="Mult")

// ✅ Calculated once:
var float threshold = 50 * input.int(defval=2, title="Mult")
```

### Explicit Types

Always define types explicitly: `float`, `int`, `bool`, `string`.

---

## 3. Code Review Checklist

Before delivering code, verify:

**Correctness:**

- [ ] All variables defined before use
- [ ] All functions are valid v6 functions
- [ ] Types match expectations

**Repainting:**

- [ ] `request.security()` uses safe lookahead
- [ ] Historical data offset correctly (`[1]`)

**Strategy Logic:**

- [ ] Entry conditions match spec
- [ ] Uses `strategy.exit()` for TP/SL

---

## 4. Essential Functions Reference

See `resources/pinescript-v6-essentials.md` for complete reference.

**Quick lookup:**

| Category | Common Functions |
|----------|-----------------|
| Indicators | `ta.sma`, `ta.ema`, `ta.rsi`, `ta.atr`, `ta.macd` |
| Signals | `ta.crossover`, `ta.crossunder` |
| Strategy | `strategy.entry`, `strategy.exit`, `strategy.close` |
| Data | `request.security`, `request.financial` |
| Input | `input.int`, `input.float`, `input.bool`, `input.source` |

---

## 5. Official Documentation Links

- **v6 Reference:** <https://www.tradingview.com/pine-script-reference/v6/>
- **User Manual:** <https://www.tradingview.com/pine-script-docs/>
- **Migration Guide:** <https://www.tradingview.com/pine-script-docs/migration-guides/>

---

## 6. Common Patterns

See `resources/common-patterns.pine` for templates.

---

## Cross-References

- **Input:** Expects `strategy-spec.md` from `planning-trading-systems`
- **Optional:** Use `teaching-pinescript` for verbose explanations
