# Scoring Rubric (100 points total, 115 with --strict)

## Contents
- 1. Brevity & Signal Density (25 points)
- 2. Commands Section (20 points)
- 3. Architecture & Patterns (20 points)
- 4. Testing Section (10 points)
- 5. Gotchas & Warnings (10 points)
- 6. Style & Conventions (10 points)
- 7. Optional Checks (--strict only, 15 bonus points)
- 8. Anti-Pattern Penalties (up to -20 points)
- Grade Scale

---

## 1. Brevity & Signal Density (25 points)

| Criterion | Points | Deduction trigger |
|---|---|---|
| Under 150 lines in root file | 5 | Over 150 lines |
| Every line passes "would removing this cause mistakes?" test | 10 | Any line that is generic, obvious, or inferable from code |
| Bullet points over paragraphs | 5 | Paragraphs where bullets would work |
| No duplicated README content | 5 | Content already in README repeated here |

**Ref**: [Official Best Practices](https://code.claude.com/docs/en/best-practices) — "Keep it concise. For each line, ask: 'Would removing this cause Claude to make mistakes?' If not, cut it."

## 2. Commands Section (20 points)

| Criterion | Points | Deduction trigger |
|---|---|---|
| Build command present and correct | 4 | Missing or wrong |
| Test command (full suite) | 4 | Missing or wrong |
| Single test / focused test command | 4 | Missing — this is high-value |
| Lint/format command | 4 | Missing or wrong |
| Pre-commit workflow (what to run before committing) | 4 | Missing |

**Ref**: [Builder.io Guide](https://www.builder.io/blog/claude-md-guide) — Commands are "among the highest-value items."

## 3. Architecture & Patterns (20 points)

| Criterion | Points | Deduction trigger |
|---|---|---|
| Package/module structure with relationships | 6 | Missing or just a file tree without explanations |
| Data flow description | 4 | Missing (how do components connect?) |
| Key design patterns with file references | 6 | Patterns described without pointing to canonical examples |
| Domain terminology mapped to code | 4 | Domain terms used without definition |

**Ref**: [HumanLayer Guide](https://www.humanlayer.dev/blog/writing-a-good-claude-md) — "Prefer pointers to copies — use file:line references instead of embedding code snippets."

## 4. Testing Section (10 points)

| Criterion | Points | Deduction trigger |
|---|---|---|
| Test framework identified | 2 | Missing |
| Test organization/conventions | 3 | Missing (labels, naming, location) |
| Mock strategy documented | 3 | Missing (hand-written vs generated, where they live) |
| Integration test requirements | 2 | Missing (Docker, env vars, etc.) |

## 5. Gotchas & Warnings (10 points)

| Criterion | Points | Deduction trigger |
|---|---|---|
| At least 2 non-obvious project-specific gotchas | 5 | Generic warnings or none |
| Cross-system dependencies documented | 3 | E.g., IPC contracts between services not mentioned |
| Platform-specific behavior noted | 2 | E.g., Linux-only features with stubs |

**Ref**: [Arize Blog](https://arize.com/blog/claude-md-best-practices-learned-from-optimizing-claude-code-with-prompt-learning/) — "Avoid negative-only constraints. Always provide an alternative."

## 6. Style & Conventions (10 points)

| Criterion | Points | Deduction trigger |
|---|---|---|
| Code style rules that differ from language defaults | 5 | Standard conventions that Claude already knows |
| Lint config referenced (not duplicated) | 5 | Full lint config pasted instead of pointing to file |

## 7. Optional Checks (--strict only, 15 bonus points)

Only scored when `--strict` is passed. When active, the total becomes 115 and target scores scale proportionally.

| Criterion | Points | Deduction trigger |
|---|---|---|
| Commit message format with examples | 3 | Missing or vague |
| PR/branch naming conventions | 2 | Missing |
| Over 300 lines | -10 | Guaranteed instruction dilution |

## 8. Anti-Pattern Penalties (up to -20 points)

| Anti-pattern | Penalty | Description |
|---|---|---|
| Generic advice | -3 each | "Write clean code", "Add tests", "Handle errors" |
| Embedded code blocks >10 lines | -3 each | Use file references instead |
| Negative-only constraints | -2 each | "Never do X" without alternative |
| Task-specific instructions | -3 each | Content relevant only to specific tasks |
| Stale/wrong information | -5 each | Commands that don't work, wrong file paths |
| Duplicates linter/formatter rules | -3 | Rules already enforced by tooling |

---

## Grade Scale

| Grade | Score (standard) | Score (--strict) |
|---|---|---|
| A++ | 100/100 | 115/115 |
| A+ | 95-99 | 109-114 |
| A | 90-94 | 104-108 |
| B+ | 85-89 | 98-103 |
| B | 80-84 | 92-97 |
| C or below | <80 | <92 |
