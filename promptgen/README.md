# promptgen

Turn rough instructions into optimized, evidence-based AI prompts. Built on research from 35+ academic papers, Anthropic/OpenAI vendor docs, and Mollick/Wharton Prompting Science Reports.

## What it does

You describe what you want a prompt to do in plain language. The skill generates a well-structured prompt following evidence-based principles - proper structure order, concise identity sections, positive framing, appropriate security patterns, and no common anti-patterns. Output goes to clipboard by default.

Supports three target formats: Claude (XML tags), GPT (final reminders), and generic (Markdown-only).

## Installation

```bash
# Via marketplace
/plugin install sai/promptgen

# Local development
claude --plugin-dir /path/to/sai/promptgen
```

## Usage

```
/promptgen <instructions> [--for claude|gpt|generic] [--verbose] [--no-copy] [--with-examples]
```

| Flag              | Default | Purpose                                          |
|:------------------|:--------|:-------------------------------------------------|
| (positional)      | -       | Rough instructions for what the prompt should do |
| `--for <model>`   | claude  | Target: claude, gpt, generic                     |
| `--verbose`       | off     | Show reasoning behind prompt decisions           |
| `--no-copy`       | off     | Output to chat only, skip clipboard              |
| `--with-examples` | off     | Include few-shot examples in generated prompt    |

## Examples

```bash
# Basic prompt generation (copies to clipboard)
/promptgen write technical docs for the auth module API endpoints

# GPT-targeted prompt
/promptgen refactor the database layer to use connection pooling --for gpt

# See the reasoning behind decisions
/promptgen --verbose investigate auth bypass vulnerabilities in the login flow

# Output to chat only, no clipboard
/promptgen --no-copy create a plan for migrating from REST to GraphQL

# Include few-shot examples in the generated prompt
/promptgen build a customer support chatbot that handles returns --with-examples
```

## How it works

The skill runs 6 phases:

1. **Input parsing** - extracts flags and instructions from arguments
2. **Task analysis** - categorizes the task, detects system vs task prompt, reads evidence-based principles
3. **Security assessment** - checks if the use case involves untrusted input, applies defensive patterns only when warranted
4. **Prompt generation** - builds the prompt using the target template (Claude/GPT/generic), applies humanize rules
5. **Self-check** - verifies against 12 anti-pattern checks with evidence citations, revises if any fail
6. **Output** - displays in fenced code block, copies to clipboard

## Research basis

The reference materials are condensed from:
- 35+ academic papers (Mollick/Wharton Reports, EMNLP, NeurIPS, ICLR, TACL publications)
- Anthropic prompt engineering docs (Claude 4.6 best practices, context engineering)
- OpenAI prompting guides (GPT-4.1 through GPT-5.2)
- Prompt injection defense research (OWASP, NIST, MITRE ATLAS)
- SPRIG genetic prompt optimization results
- Chroma context rot research

## Requirements

- macOS (pbcopy) or Linux (xclip/xsel) for clipboard support
- Clipboard is optional - prompts are always displayed in chat
