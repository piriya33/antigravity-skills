---
name: use-tradingview-mcp
description: Instructions for enabling and using the local TradingView MCP server for real-time market screening and analysis.
---

# Skill: Using TradingView MCP

This set of tools allows Antigravity to interact effectively with the locally hosted `tradingview-mcp` server.

## 1. Installation & Configuration

To enable this server in your Cursor/Claude environment, you must add it to your MCP configuration file.

**Config Location:**
- **Cursor:** `.cursor/mcp.json` (Project Root) OR Global Settings.
- **Claude Desktop:** `~/Library/Application Support/Claude/claude_desktop_config.json`

**JSON Snippet:**
Copy and paste this into your `mcpServers` block:

```json
{
  "tradingview-local": {
    "command": "/Users/piriyasambandaraksa/.local/bin/uv",
    "args": [
      "run",
      "python",
      "src/tradingview_mcp/server.py"
    ],
    "cwd": "/Users/piriyasambandaraksa/Dropbox/Antigravity/The Dojo/servers/tradingview-mcp",
    "env": {
        "PYTHONUNBUFFERED": "1"
    }
  }
}
```

## 2. Capabilities & Prompts

Once connected, I (the Agent) can use the following capabilities. You can prompt me naturally:

### Market Screening
- **"Find top gainers/losers"**
    - *Example:* "Show me the top 5 crypto gainers on Binance today."
    - *Tool:* `screen_market` (Gainers/Losers/Volume/Volatility)
- **"Scan for Bollinger Squeeze"**
    - *Example:* "Scan for coins with tight Bollinger Bands ready to breakout."
    - *Tool:* `bollinger_scan`

### Technical Analysis
- **"Analyze this chart"**
    - *Example:* "Analyze BTCUSDT on 4h timeframe."
    - *Tool:* `get_technical_analysis` (Returns RSI, MACD, MA, etc.)
- **"Check rating"**
    - *Example:* "Is TSLA a buy right now according to Bollinger logic?"
    - *Tool:* `get_rating`

## 3. Best Practices

- **Use Specific Tickers:** Always specify the exchange if possible (e.g., `BINANCE:BTCUSDT` or `NASDAQ:AAPL`).
- **Combine with Skills:**
    - Use `tradingview-mcp` to **fetch data/alerts**.
    - Use `expert-elliott-wave` to **analyze the structure** of those alerts.
    - Example: _"Scan for top gainers, then perform a fractal Elliott Wave count on the top result."_
