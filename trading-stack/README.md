# Trading Stack

A complete workflow for developing TradingView Pine Script trading systems with AI assistance.

## Skills Overview

| Skill | Purpose | When to Use |
|-------|---------|-------------|
| **planning-trading-systems** | Structured strategy design | Before writing any code |
| **coding-pinescript** | Pine Script v6 development | Writing/debugging scripts |
| **analyzing-backtests** | Interpret strategy results | After running backtests |
| **iterating-strategies** | Version tracking | When making changes |
| **use-tradingview-mcp** | Market data access | For screeners/analysis |

## Recommended Workflow

```
1. Planning → Design your strategy logic and rules
      ↓
2. Coding → Implement in Pine Script v6
      ↓
3. Backtesting → Run in TradingView Strategy Tester
      ↓
4. Analyzing → Evaluate results with the skill
      ↓
5. Iterating → Log changes, measure impact
      ↓
   (repeat until satisfied)
```

## Quick Start

1. Copy this folder to your project's `.agent/skills/trading-stack/`
2. When working on trading systems, the AI will:
   - Follow `planning-trading-systems` for design
   - Use `coding-pinescript` for implementation (includes v6 reference docs)
   - Apply `analyzing-backtests` for result interpretation
   - Enforce `iterating-strategies` for version discipline

## Skill Details

### planning-trading-systems

Generates a structured `strategy-spec.md` before any coding. Ensures you think through:

- Entry/exit rules
- Risk management
- Edge hypothesis
- Testing approach

### coding-pinescript

Embedded Pine Script v6 reference documentation to prevent hallucination. Includes:

- Type system guide (series vs simple)
- Common patterns library
- Anti-pattern warnings

### analyzing-backtests

Semantic parsing of TradingView backtest results. Provides:

- Quality scoring (profit factor, drawdown analysis)
- Red flag detection (overfitting, curve fitting)
- Actionable recommendations

### iterating-strategies

Version control for strategy development:

- Changelog template
- One-change-at-a-time enforcement
- Regression warnings

### use-tradingview-mcp

Instructions for using the TradingView MCP server for:

- Market screening
- Technical indicator data
- Multi-exchange support
