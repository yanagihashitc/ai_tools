---
name: pr-review-fix
description: Pre-PR review with auto-fix for minor issues (major issues go to handoff)
---

# PR Pre-Review Agent

Review diff before creating a PR. **Fix minor issues directly**, **record major issues in handoff**.

## Step 1: Get Diff to Review (apps/**/src only)

```bash
# List commits since branching from main
git log main..HEAD --oneline

# Full diff against main for apps/**/src
git diff main...HEAD -- apps/api/src

# Also check for uncommitted changes in apps/**/src
git diff --cached -- apps/api/src
git diff -- apps/api/src
```

If no diff exists, report "No diff to review" and exit.

**Note**: This command reviews diffs against `main` and only under `apps/api/src`.

## Step 2: Review the Diff

Check the following aspects:

### 🐛 Potential Bugs
- Logic errors, unhandled edge cases, NullPointer, type errors

### 🔒 Security Concerns
- Vulnerabilities, insufficient input validation, credential exposure, path traversal

### ⚡ Performance Issues
- Inefficient algorithms, N+1 problems, unnecessary processing

### 📖 Readability/Maintainability
- Poor naming, overly complex logic, missing comments

### 🧪 Test Coverage
- Missing tests, untested boundary values, insufficient error case coverage

### 📋 Playbook Check (Required)
- Cross-check against active entries in `@apps/api/agents/handoffs/review_playbook.md`
- Report any matching items

## Step 3: Classify Issues and Take Action

Classify detected issues by the following criteria:

### 🟢 Minor (Fix immediately)
- Typos, comment fixes
- Obvious bugs (off-by-one, missing null check, etc.)
- Log message improvements
- Import cleanup
- Simple refactoring (variable name improvements, deduplication, etc.)

**→ Apply fix immediately with `apply_patch`**

### 🟡 Medium (Judge and fix)
- Adding validation
- Error handling improvements
- Small logic fixes

**→ Apply if fix is clear; otherwise write to handoff**

### 🔴 Major (Record in handoff)
- Design changes required
- Significant security implementation changes
- Extensive test additions
- Multi-file changes
- Specification confirmation needed

**→ Do not fix; record in review-complete.md**

## Step 4: Output

### When fixes are applied

Report each fix briefly:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ Fix applied: {filename}:{line}
Category: {Bug|Security|Performance|Readability}
Content: {one-line description of fix}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### When major issues are detected

Record in `apps/api/agents/handoffs/review-complete.md` using the following format:

**Return To decision criteria:**

| Issue Type | Return To | Output File |
|------------|-----------|-------------|
| Behavior changes (including error message changes) | Test Agent | `orchestrator-test.md` |
| Security implementation | Implementation Agent | `test-impl.md` |
| Idempotency implementation | Implementation Agent | `test-impl.md` |
| Code quality | Refactor Agent | `impl-refactor.md` |
| Missing tests | Test Agent | `orchestrator-test.md` |

```markdown
# Handoff: PR Pre-Review → Orchestrator (Fix Request)

## Review Results

**PR Pre-Review Agent**: ⚠️ CHANGES REQUESTED

**Final Status**: ⚠️ CHANGES REQUESTED

Backflow: → [Target Agent] (overwrite [handoff].md)

## Issues Found

| # | Location | Problem | Priority | Return To |
|---|----------|---------|----------|-----------|
| 1 | src/xxx.py:45 | {problem summary} | High | {Agent name} |

## Issue Details

### 1. {Issue Title}
- **Location**: `src/xxx.py:45`
- **Problem**: {detailed explanation}
- **Fix Method**: {suggested fix}
- **Reference**: {related docs if any}

## Post-Fix Verification

1. Fix in specified Agent
2. Verify code quality in Refactor Agent
3. Restart from Review Agent 1
4. Final verification in Review Agent 2

## Fix Completion Check (fill after fixes)

- [ ] Fixed issue #1
- [ ] Added tests (if needed)
- [ ] Refactor Agent completed
- [ ] Review Agent 1 approved
```

### When no issues found

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ PR Ready

Review target: {git diff range}
Issues found: None (or minor fixes already applied)

Ready to create PR.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## Step 5: Summary

Output the following at the end:

```
## PR Pre-Review Summary

| Classification | Count | Action |
|----------------|-------|--------|
| 🟢 Minor (fixed) | X | Applied |
| 🟡 Medium (fixed) | X | Applied |
| 🔴 Major (needs action) | X | Recorded in handoff |

{If major issues exist}
⚠️ Major fixes required. Check review-complete.md and run the agent cycle.

{If no issues}
✅ Ready to create PR.
```
