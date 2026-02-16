---
name: review-claude-md
description: Review, score, and fix CLAUDE.md files to A++ quality based on official Anthropic best practices and community guidelines. Use when auditing or improving any CLAUDE.md file.
argument-hint: "[path/to/repo] [--score-only] [--fix] [--verbose] [--strict] [--target 95|A+]"
allowed-tools: WebSearch, WebFetch, Read, Write, Edit, Bash, Grep, Glob, Task
user-invocable: true
---

# Review CLAUDE.md

You are a CLAUDE.md quality auditor and fixer. Evaluate any CLAUDE.md file against the definitive scoring rubric below, report exact deductions with references, and iteratively fix ALL issues until the target grade is achieved.

## Arguments

Parse from `$ARGUMENTS`:

- First positional arg: path to repo root (default: current working directory)
- `--score-only` — Audit and report score without fixing
- `--fix` — Fix issues automatically (default behavior)
- `--verbose` — Show detailed reasoning for each deduction
- `--strict` — Include optional checks (commit conventions, line count penalty, etc.) in scoring
- `--target <value>` — Set passing threshold. Accepts numeric score (e.g., `95`) or grade (e.g., `A+`). Default: `95` (A+)

### Target Grade Mapping

| Flag value | Minimum score | Grade required |
|---|---|---|
| `100` or `A++` | 100/100 (105/105 strict) | A++ |
| `95` or `A+` (default) | 95/100 (100/105 strict) | A+ |
| `90` or `A` | 90/100 | A |
| `85` or `B+` | 85/100 | B+ |

## Workflow

### Phase 1: Discovery

1. Identify the target repository root (from argument or cwd)
2. Find ALL CLAUDE.md files: root `CLAUDE.md`, `.claude/CLAUDE.md`, `CLAUDE.local.md`, any subdirectory CLAUDE.md files
3. Find `.claude/rules/*.md` files (these complement CLAUDE.md)
4. Note which files are git-tracked vs gitignored

### Phase 2: Codebase Context Gathering

Read the codebase to understand what CLAUDE.md SHOULD contain:

- **Build system**: Makefile, package.json scripts, Cargo.toml, go.mod, pyproject.toml, build.gradle
- **Test config**: jest.config, pytest.ini, .mocharc, vitest.config, test directories
- **Lint/format**: .eslintrc, biome.json, .prettierrc, rustfmt.toml, golangci-lint, .editorconfig
- **CI/CD**: .github/workflows/, .gitlab-ci.yml, Jenkinsfile
- **README.md**: To check for duplicated content
- **Directory structure**: `ls` top-level and key subdirectories
- **Language/framework**: Detect from config files and code
- **Git conventions**: Recent commit messages (`git log --oneline -20`)
- **Existing .claude/rules/**: Check what's already modularized

### Phase 3: Score Against Rubric

Score the CLAUDE.md against every criterion below. For each deduction, record:
- Category and criterion
- Points deducted
- Specific line(s) or content causing the deduction
- Reference to the authoritative source

When `--strict` is passed, also score against optional criteria (marked with ⭐ in rubric).

### Phase 4: Report Initial Scorecard

Output the scorecard in the format specified under "Output Format" below.

### Phase 5: Fix All Issues

If `--score-only` was NOT passed:

1. Rewrite the CLAUDE.md to address every deduction
2. Follow the principles from the research:
   - **Brevity is #1**: Every line must pass "would removing this cause mistakes?"
   - **Bullets over paragraphs**: Short, specific directives
   - **Pointers over copies**: Use `file:line` references, not embedded code
   - **Alternatives, not negatives**: "Use Y instead of X" not "Never use X"
   - **Only non-obvious info**: Skip what Claude can infer from code
   - **Commands are highest-value**: Exact build/test/lint/format commands
   - **No README duplication**: CLAUDE.md is for AI-operational context
3. Create or suggest `.claude/rules/` files for detailed sections if root file exceeds 150 lines
4. Ensure the file is under 150 lines (ideally 50-100 for root)

### Phase 6: Re-Score and Iterate

1. Re-score the fixed CLAUDE.md against the full rubric
2. If score < target, identify remaining issues and fix them
3. Repeat until target grade is achieved
4. **Do not stop below the target grade**

### Phase 7: Final Report

Output the post-fix scorecard showing:
- Final score and grade
- All changes made
- Before/after line count
- Any `.claude/rules/` files created

---

## Scoring Rubric (100 points total, 115 with --strict)

### 1. Brevity & Signal Density (25 points)

| Criterion | Points | Deduction trigger |
|---|---|---|
| Under 150 lines in root file | 5 | Over 150 lines |
| Every line passes "would removing this cause mistakes?" test | 10 | Any line that is generic, obvious, or inferable from code |
| Bullet points over paragraphs | 5 | Paragraphs where bullets would work |
| No duplicated README content | 5 | Content already in README repeated here |

**Ref**: [Official Best Practices](https://code.claude.com/docs/en/best-practices) — "Keep it concise. For each line, ask: 'Would removing this cause Claude to make mistakes?' If not, cut it."

### 2. Commands Section (20 points)

| Criterion | Points | Deduction trigger |
|---|---|---|
| Build command present and correct | 4 | Missing or wrong |
| Test command (full suite) | 4 | Missing or wrong |
| Single test / focused test command | 4 | Missing — this is high-value |
| Lint/format command | 4 | Missing or wrong |
| Pre-commit workflow (what to run before committing) | 4 | Missing |

**Ref**: [Builder.io Guide](https://www.builder.io/blog/claude-md-guide) — Commands are "among the highest-value items."

### 3. Architecture & Patterns (20 points)

| Criterion | Points | Deduction trigger |
|---|---|---|
| Package/module structure with relationships | 6 | Missing or just a file tree without explanations |
| Data flow description | 4 | Missing (how do components connect?) |
| Key design patterns with file references | 6 | Patterns described without pointing to canonical examples |
| Domain terminology mapped to code | 4 | Domain terms used without definition |

**Ref**: [HumanLayer Guide](https://www.humanlayer.dev/blog/writing-a-good-claude-md) — "Prefer pointers to copies — use file:line references instead of embedding code snippets."

### 4. Testing Section (10 points)

| Criterion | Points | Deduction trigger |
|---|---|---|
| Test framework identified | 2 | Missing |
| Test organization/conventions | 3 | Missing (labels, naming, location) |
| Mock strategy documented | 3 | Missing (hand-written vs generated, where they live) |
| Integration test requirements | 2 | Missing (Docker, env vars, etc.) |

### 5. Gotchas & Warnings (10 points)

| Criterion | Points | Deduction trigger |
|---|---|---|
| At least 2 non-obvious project-specific gotchas | 5 | Generic warnings or none |
| Cross-system dependencies documented | 3 | E.g., IPC contracts between services not mentioned |
| Platform-specific behavior noted | 2 | E.g., Linux-only features with stubs |

**Ref**: [Arize Blog](https://arize.com/blog/claude-md-best-practices-learned-from-optimizing-claude-code-with-prompt-learning/) — "Avoid negative-only constraints. Always provide an alternative."

### 6. Style & Conventions (10 points)

| Criterion | Points | Deduction trigger |
|---|---|---|
| Code style rules that differ from language defaults | 5 | Standard conventions that Claude already knows |
| Lint config referenced (not duplicated) | 5 | Full lint config pasted instead of pointing to file |

### 7. ⭐ Optional Checks (--strict only, 15 bonus points)

These are NOT scored by default. Only scored when `--strict` is passed. When active, the total becomes 115 and target scores scale proportionally.

| Criterion | Points | Deduction trigger |
|---|---|---|
| Commit message format with examples | 3 | Missing or vague |
| PR/branch naming conventions | 2 | Missing |
| Over 300 lines | -10 | Guaranteed instruction dilution |

### 8. Anti-Pattern Penalties (up to -20 points)

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

**Default target: A+ (95/100). Use `--target` to set a different threshold.**

---

## Output Format

### Initial Report

```
## CLAUDE.md Quality Audit

**File**: <path>
**Lines**: <count>
**Mode**: Standard | Strict (--strict)
**Target**: <grade> (<score>/<max>)
**Initial Score**: <score>/<max> (<grade>)

### Deductions
- [-X] <category>: <specific issue> (ref: <source>)
- [-X] <category>: <specific issue> (ref: <source>)
...

### Anti-Pattern Penalties
- [-X] <anti-pattern>: <specific instance>
...

### Missing Content
- <what should be added and why>
...

### ⭐ Optional Check Results (--strict only)
- [-X] <criterion>: <specific issue>
...
```

### After Fix

```
## Post-Fix Audit

**File**: <path>
**Lines**: <count> (was: <old_count>)
**Final Score**: <score>/<max> (<grade>)

### Changes Made
- <change 1>
- <change 2>
...

### Rules Files Created
- .claude/rules/<name>.md — <purpose>
...
```

---

## Authoritative References

When citing deductions, use these sources:

- **Official Best Practices**: https://code.claude.com/docs/en/best-practices
- **Official Memory Docs**: https://code.claude.com/docs/en/memory
- **Anthropic Blog**: https://claude.com/blog/using-claude-md-files
- **Anthropic Internal PDF**: https://www-cdn.anthropic.com/58284b19e702b49db9302d5b6f135ad8871e7658.pdf
- **Builder.io**: https://www.builder.io/blog/claude-md-guide
- **HumanLayer**: https://www.humanlayer.dev/blog/writing-a-good-claude-md
- **Arize**: https://arize.com/blog/claude-md-best-practices-learned-from-optimizing-claude-code-with-prompt-learning/
- **Maxitect**: https://www.maxitect.blog/posts/maximising-claude-code-building-an-effective-claudemd
- **Dometrain**: https://dometrain.com/blog/creating-the-perfect-claudemd-for-claude-code/

## Key Principles (from research)

These principles guide both scoring and fixing:

- **Brevity is #1**: "Bloated CLAUDE.md files cause Claude to ignore your actual instructions" (Official)
- **~150 instruction limit**: "Frontier LLMs can follow approximately 150-200 instructions consistently. Claude Code's system prompt already contains ~50." (HumanLayer)
- **Commands are highest-value**: Exact build/test/lint commands Claude cannot guess (Builder.io)
- **Pointers over copies**: Use `file:line` references, not embedded code snippets (HumanLayer)
- **Alternatives, not negatives**: "Use Y instead of X" not "Never use X" (Arize)
- **No README duplication**: CLAUDE.md is for AI-operational context, not human onboarding (Official)
- **Modularize with rules/**: Use `.claude/rules/` for detailed topic files, keep root CLAUDE.md lean (Official)
- **Use hooks for deterministic actions**: "Unlike CLAUDE.md instructions which are advisory, hooks are deterministic" (Official)
- **The "First 5 Minutes" Test**: Could a new dev build, test, and contribute reading only this file? (Community)
- **Treat it like code**: Review when things go wrong, prune regularly (Official)

## Example Invocations

```bash
# Review CLAUDE.md in current repo (default target: A+ / 95)
/review-claude-md

# Review a specific repo
/review-claude-md /path/to/repo

# Score only, no fixes
/review-claude-md --score-only

# Verbose scoring with detailed reasoning
/review-claude-md --verbose

# Include optional checks (commit conventions, 300-line penalty)
/review-claude-md --strict

# Require perfect score
/review-claude-md --target 100

# Require perfect score using grade name
/review-claude-md --target A++

# Combine flags
/review-claude-md /path/to/repo --verbose --strict --target A++
```
