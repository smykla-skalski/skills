# Output Format Templates

## Initial Report

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

## After Fix

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
