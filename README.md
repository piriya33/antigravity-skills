# Antigravity Skills

A collection of AI skill stacks for specialized domains. Each stack contains skills that work together to guide AI assistants through complex workflows.

## What Are Skills?

Skills are instruction files that teach AI assistants (Claude, Gemini, etc.) how to approach specific tasks. They provide:

- Domain expertise and best practices
- Structured workflows
- Common patterns and templates
- Error prevention and quality checks

## Available Stacks

### 🎯 [Trading Stack](./trading-stack/)

Pine Script and TradingView development workflow:

- `planning-trading-systems` - Design before coding
- `coding-pinescript` - Write/debug Pine Script v6
- `analyzing-backtests` - Interpret strategy results
- `iterating-strategies` - Track versions, prevent regression
- `use-tradingview-mcp` - TradingView data integration

### 📝 Content Stack (Coming Soon)

Article writing, presentations, translations

### 🔬 Research Stack (Coming Soon)

Analysis, reports, data synthesis

## How to Use

### Option 1: Copy to Your Project

Copy the relevant stack folder into your project's `.agent/skills/` directory.

### Option 2: Reference Directly

Point your AI assistant to read the SKILL.md files from this repo.

## Architecture

See [AGENT.md](./AGENT.md) for the 3-layer architecture these skills follow:

- **Directive Layer**: Skills define what to do
- **Orchestration Layer**: AI makes decisions
- **Execution Layer**: Deterministic scripts do the work

## Contributing

Each skill follows this structure:

```
skill-name/
├── SKILL.md          # Main instruction file
└── resources/        # Templates, examples, reference docs
```

## License

MIT
