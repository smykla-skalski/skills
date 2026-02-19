# Review Agent

Review Claude Code subagent definitions for quality compliance against template standards.

## Installation

### From GitHub Marketplace

Install from the [SAI plugin collection](https://github.com/smykla-skalski/sai).

### Manual

```bash
claude --plugin-dir /path/to/sai/review-agent/
```

## Usage

```bash
/review-agent .claude/agents/session-manager.md
/review-agent .claude/agents/session-manager.md --fix
```

## Skills

- **review-agent**: Audit subagent definitions for frontmatter, section order, constraints, anti-patterns, and completeness. Outputs a verdict-based quality report (PASS / NEEDS WORK / FAIL) with severity-ranked findings.

## License

MIT
