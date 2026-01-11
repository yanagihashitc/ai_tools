---
name: refactor-test-verify
description: Orchestrates a 3-stage pipeline (Refactor → Tests → Verify) to safely refactor code with minimal tests and verification steps. Keeps changes small and reversible.
metadata:
  author: Team
  version: "1.0"
  type: pipeline
---

# Refactor-Test-Verify Pipeline

You are an orchestrator that runs a 3-stage pipeline with strictly separated outputs.

## When to Use This Skill

- When performing a complete refactoring workflow
- When you need refactoring with test coverage and verification in one pass
- For safe, documented code improvements

## Hard Rules

- Keep changes small and reversible
- Do NOT change external behavior unless explicitly requested
- Do not introduce new dependencies
- If uncertain, preserve behavior conservatively and state assumptions

## Steps

1. **Determine language/framework/test runner** from repo files (pyproject.toml, package.json, etc.)
2. **Stage 1: Refactor** - Apply safe refactoring principles
3. **Stage 2: Tests** - Add 3-8 minimal, high-value tests
4. **Stage 3: Verify** - Provide verification steps
5. **Keep each stage minimal and reversible** - one theme per patch

## Output Format (Must Follow Exactly)

```
Active Skill: refactor-test-verify

## Stage 0: Assumptions
- ...

## Stage 1: Refactor (diff)
### Plan
- ...
### Patch
\`\`\`diff
...
\`\`\`
### Notes
- ...

## Stage 2: Tests (diff)
### Test Plan
- ...
### Patch
\`\`\`diff
...
\`\`\`
### Notes
- ...

## Stage 3: Verify (markdown)
### Commands
1. ...
### Expected results
- ...
### What to watch
- ...
### Rollback
- ...
```

## Checklist

- [ ] Stage outputs are separated
- [ ] No external behavior change (unless requested)
- [ ] No new dependencies
- [ ] Tests: 3-8, high value, minimal scaffolding
- [ ] Verify includes commands, expected signals, rollback

## Related Skills

This pipeline combines three core skills:
- `refactor-safe` - Stage 1
- `testgen-pytest` - Stage 2
- `verify-local` - Stage 3
