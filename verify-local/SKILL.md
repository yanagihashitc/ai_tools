---
name: verify-local
description: Provides a verification plan runnable on a developer machine with exact commands, expected results, and rollback guidance.
metadata:
  author: Team
  version: "1.0"
---

# Local Verification Plan

You provide a verification plan runnable on a developer machine.

## When to Use This Skill

- After making code changes that need verification
- When setting up a test/validation workflow
- When documenting how to verify a refactor or feature

## Hard Rules

- Provide exact commands
- Include expected signals of success/failure
- Include rollback guidance

## Steps

1. **Identify commands from repo** - pyproject.toml, Makefile, package.json, scripts
2. **Provide minimal run sequence:**
   - Formatter/linter/typecheck if present
   - Unit tests (pytest, jest, etc.)
   - Optional smoke test (run app, call a simple endpoint)
3. **Provide rollback steps** - git revert/checkout

## Output Format

- **Commands**
- **Expected results**
- **What to watch** (regressions)
- **Rollback steps**
- **Assumptions**

## Checklist

- [ ] Commands listed
- [ ] Expected results included
- [ ] Rollback included
- [ ] Assumptions stated

## Example

### Commands
```bash
# 1. Format check
ruff check .

# 2. Type check
mypy src/

# 3. Run tests
pytest tests/ -v

# 4. Smoke test (optional)
python -m src.main --dry-run
```

### Expected Results
- ruff: No errors
- mypy: 0 errors found
- pytest: All tests pass (green)
- Smoke test: Exits with code 0

### What to Watch
- Any new test failures
- Type errors in modified files
- Runtime errors in smoke test

### Rollback
```bash
git checkout HEAD~1 -- <modified_files>
# or
git revert HEAD
```

### Assumptions
- Python 3.10+ installed
- Dependencies installed via `pip install -e ".[dev]"`
