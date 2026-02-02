---
name: pr-review-fix_web
description: Web PR pre-review with auto-fix for minor issues (major issues go to Web agents handoff)
---

# PR Pre-Review Agent (Web)

Review diff before creating a PR for **Web (Next.js + TypeScript)**.

- **Fix minor issues directly**
- **Record major issues in a Web handoff**, so the SDD/TDD agent cycle can pick them up

## Step 1: Get Diff to Review (Web scope)

```bash
# List commits since branching from main
git log main..HEAD --oneline

# Full diff against main (Web)
git diff main...HEAD -- apps/web

# Also check for uncommitted changes (Web)
git diff --cached -- apps/web
git diff -- apps/web
```

If no diff exists, report "No diff to review" and exit.

## Step 2: Review the Diff (Web-specific checklist)

### 🐛 Potential Bugs
- Logic errors, unhandled edge cases, type errors, state desync, loading/error branching mistakes

### 🔒 Security / Data Safety
- Credential exposure, unsafe rendering (XSS), unsafe URL handling, missing input validation, leaking server-only data into client

### ⚡ Performance
- Unnecessary clientization (`use client` too broad), huge dependency/bundle impact, infinite re-render loops, expensive derived state

### 📖 Readability / Maintainability
- Poor naming, overly complex components/hooks, missing boundaries between UI and logic, duplicated patterns

### 🧪 Test Coverage / Testability
- Missing unit tests for hooks/utils
- E2E fragility: missing stable selectors (`role/name` preferred, or `data-testid`)

### ✅ Pre‑E2E Integrated Readiness (Required)
Cross-check against the fixed checklist in:
- `@apps/web/agents/agent_design/10_pre_e2e_review_agent.md`

## Step 3: Classify Issues and Take Action

### 🟢 Minor (Fix immediately)
- Typos, comment fixes, import cleanup
- Obvious bugfix (null guard, missing dependency array, etc.)
- Small refactor (naming, tiny dedup) with minimal risk

**→ Apply fix immediately with `apply_patch`**

### 🟡 Medium (Judge and fix if clearly correct)
- Error handling improvements
- Small logic fixes with clear intent
- Adding stable selectors (`role/name` or `data-testid`) when obviously needed for E2E

**→ Apply if safe; otherwise record in handoff**

### 🔴 Major (Record in handoff)
- Design changes required
- Next.js boundary violations (Server Component using hooks/browser APIs)
- Architecture issues (`use client` sprawl, fetch cache strategy inconsistency)
- Extensive test additions (unit/E2E)
- Multi-file changes with non-trivial risk

**→ Do not fix; record in `apps/web/agents/handoffs/review-complete.md`**

## Step 4: Output

### When fixes are applied

Report each fix briefly:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ Fix applied: {filename}:{line}
Category: {Bug|Security|Performance|Readability|Testability}
Content: {one-line description of fix}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### When major issues are detected (Web handoff)

Record in `apps/web/agents/handoffs/review-complete.md`.

**Return To decision criteria (Web agents):**

| Issue Type | Return To | Target Handoff | Suggested Resume |
|------------|-----------|----------------|------------------|
| Story/spec mismatch, missing states in stories | Story Agent | `apps/web/agents/handoffs/orchestrator-story.md` | `--resume-from story` |
| UI component behavior/props/rendering | Component Agent | `apps/web/agents/handoffs/story-component.md` | `--resume-from component` |
| Styling/a11y polish (focus, contrast, semantics) | Style Agent | `apps/web/agents/handoffs/component-style.md` | `--resume-from style` |
| Hook/API client tests missing | Test Agent | `apps/web/agents/handoffs/orchestrator-test.md` | `--resume-from test` |
| Hook/logic implementation bug | Logic Agent | `apps/web/agents/handoffs/test-logic.md` | `--resume-from logic` |
| Refactor/typing/cleanup (tests already exist) | Refactor Agent | `apps/web/agents/handoffs/logic-refactor.md` | `--resume-from refactor` |
| Pre‑E2E checklist failures (selectors, routing traceability, boundary, perf) | Pre‑E2E Review Agent | `apps/web/agents/handoffs/review-pre-e2e.md` | `--resume-from pre_e2e_review` |

```markdown
# Handoff: PR Pre-Review (Web) → Orchestrator/Agents (Fix Request)

## Review Results

**PR Pre-Review Agent**: ⚠️ CHANGES REQUESTED

## Backflow Target

**Return To**: [Story|Component|Style|Test|Logic|Refactor|Pre-E2E Review]
**Target Handoff**: `apps/web/agents/handoffs/...`
**Suggested Resume**: `cd apps/web/agents/scripts && ./run-cycle.sh --resume-from <step>`

## Issues Found

| # | Location | Problem | Priority | Return To |
|---|----------|---------|----------|-----------|
| 1 | apps/web/src/...:45 | {problem summary} | High | {Agent name} |

## Issue Details

### 1. {Issue Title}
- **Location**: `apps/web/...:45`
- **Problem**: {detailed explanation}
- **Fix Method**: {suggested fix}
- **Reference**: {related doc, e.g. `@apps/web/agents/agent_design/10_pre_e2e_review_agent.md`}

## Post-Fix Verification

- [ ] `cd apps/web && npm run lint`
- [ ] `cd apps/web && npm run type-check`
- [ ] `cd apps/web && npm run test -- --run`
```

### When no issues found

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ PR Ready (Web)

Review target: main...HEAD
Issues found: None (or minor fixes already applied)

Ready to create PR.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## Step 5: Summary

Output the following at the end:

```
## PR Pre-Review Summary (Web)

| Classification | Count | Action |
|----------------|-------|--------|
| 🟢 Minor (fixed) | X | Applied |
| 🟡 Medium (fixed) | X | Applied |
| 🔴 Major (needs action) | X | Recorded in handoff |

{If major issues exist}
⚠️ Major fixes required. Check apps/web/agents/handoffs/review-complete.md and run:
cd apps/web/agents/scripts && ./run-cycle.sh --resume-from <step>

{If no issues}
✅ Ready to create PR.
```
