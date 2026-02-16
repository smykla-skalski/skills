---
name: review-claude-md
description: Audit, score, and fix CLAUDE.md files against a 100-point rubric based on official Anthropic best practices and community guidelines. Use when the user asks to "review CLAUDE.md", "audit CLAUDE.md", "score CLAUDE.md", "improve CLAUDE.md", or "fix CLAUDE.md".
argument-hint: "[path/to/repo] [--score-only] [--fix] [--verbose] [--strict] [--target 95|A+]"
allowed-tools: WebSearch, WebFetch, Read, Write, Edit, Bash, Grep, Glob, Task
user-invocable: true
---

# Review CLAUDE.md

Audit any CLAUDE.md file against a 100-point rubric, report exact deductions with references, then iteratively fix all issues until the target grade is achieved.

## Arguments

Parse from `$ARGUMENTS`:

- First positional arg: path to repo root (default: cwd)
- `--score-only` — Report score without fixing
- `--fix` — Fix issues automatically (default)
- `--verbose` — Show detailed reasoning per deduction
- `--strict` — Include optional checks (commit conventions, line count penalty); total becomes 115
- `--target <value>` — Passing threshold as numeric (`95`) or grade (`A+`). Default: `95` (A+)

| Target | Standard | Strict |
|---|---|---|
| A++ (100) | 100/100 | 115/115 |
| A+ (95, default) | 95/100 | 109/115 |
| A (90) | 90/100 | 104/115 |
| B+ (85) | 85/100 | 98/115 |

## Workflow

### Phase 1: Discovery

1. Identify target repo root (from argument or cwd)
2. Find all CLAUDE.md files: root, `.claude/CLAUDE.md`, `CLAUDE.local.md`, subdirectories
3. Find `.claude/rules/*.md` files
4. Note git-tracked vs gitignored

### Phase 2: Codebase Context

Read the codebase to understand what CLAUDE.md SHOULD contain:

- **Build system**: Makefile, package.json, Cargo.toml, go.mod, pyproject.toml
- **Test config**: jest.config, pytest.ini, vitest.config, test directories
- **Lint/format**: .eslintrc, biome.json, .prettierrc, rustfmt.toml, golangci-lint
- **CI/CD**: .github/workflows/, .gitlab-ci.yml
- **README.md**: Check for duplicated content
- **Directory structure**: Top-level layout
- **Git conventions**: `git log --oneline -20`
- **Existing .claude/rules/**: Already modularized content

### Phase 3: Score

Score against every criterion in [rubric.md](references/rubric.md). For each deduction, record:
- Category and criterion
- Points deducted
- Specific line(s) causing deduction
- Reference from [sources.md](references/sources.md)

With `--strict`, also score optional criteria.

### Phase 4: Report

Output the initial scorecard per [output-format.md](references/output-format.md).

### Phase 5: Fix

If `--score-only` was NOT passed:

1. Rewrite the CLAUDE.md addressing every deduction
2. Apply these principles:
   - Every line must pass "would removing this cause mistakes?"
   - Bullets over paragraphs
   - `file:line` references over embedded code
   - "Use Y instead of X" over "Never use X"
   - Only non-obvious info — skip what Claude infers from code
   - Commands are highest-value items
   - No README content duplication
3. Create `.claude/rules/` files if root exceeds 150 lines
4. Target: under 150 lines (ideally 50-100 for root)

### Phase 6: Re-Score and Iterate

1. Re-score the fixed file against full rubric
2. If below target, fix remaining issues
3. Repeat until target grade is achieved — do not stop below it

### Phase 7: Final Report

Output post-fix scorecard per [output-format.md](references/output-format.md) showing:
- Final score and grade
- All changes made
- Before/after line count
- Any `.claude/rules/` files created

## Example

```bash
# Default audit (target: A+ / 95)
/review-claude-md

# Specific repo, verbose, strict, perfect score
/review-claude-md /path/to/repo --verbose --strict --target A++

# Score only
/review-claude-md --score-only
```

### Sample Audit Output

```
## CLAUDE.md Quality Audit

**File**: ./CLAUDE.md
**Lines**: 247
**Mode**: Standard
**Target**: A+ (95/100)
**Initial Score**: 68/100 (C)

### Deductions
- [-5] Brevity: 247 lines exceeds 150-line limit (ref: Official Best Practices)
- [-5] Brevity: Lines 40-55 repeat README project overview (ref: Official Best Practices)
- [-4] Commands: Missing single-test command (ref: Builder.io)
- [-4] Commands: Missing pre-commit workflow (ref: Builder.io)
- [-6] Architecture: File tree on lines 12-30 has no relationship explanations (ref: HumanLayer)
- [-4] Architecture: "handler", "service", "resolver" used without definitions (ref: HumanLayer)
- [-3] Testing: No mock strategy documented (ref: rubric)
- [-5] Gotchas: No project-specific gotchas — only generic "handle errors" (ref: Arize)

### Anti-Pattern Penalties
- [-3] Generic advice: "Always write clean, readable code" (line 89)
- [-3] Generic advice: "Make sure to add tests" (line 92)
- [-2] Negative-only: "Never use var" without alternative (line 78)

### Missing Content
- Single-test command (e.g., `npm test -- --grep "pattern"`)
- Pre-commit checklist (lint, typecheck, test)
- Mock strategy (hand-written vs auto-generated, location)
- At least 2 project-specific gotchas
```
