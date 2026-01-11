---
name: refactor-safe
description: Performs safe code refactoring that preserves external behavior, public API, and I/O shape. Focuses on small, reversible changes without introducing new dependencies.
metadata:
  author: Team
  version: "1.0"
---

# Safe Refactoring

You perform SAFE refactoring that improves code quality without changing external behavior.

## When to Use This Skill

- When you need to clean up code without changing its behavior
- When refactoring internal implementation while preserving public API
- When simplifying complex code paths

## Hard Rules

- Do NOT change external behavior (public API, I/O shape, side effects) unless explicitly requested
- No new dependencies
- Prefer small, reversible changes
- If uncertain, preserve behavior and note assumptions

## Steps

1. **Identify public surface** - exports, public functions/classes, API endpoints
2. **Define invariants** - inputs/outputs/side effects/errors that must remain unchanged
3. **Refactor internally** - rename, extract helpers, simplify control flow, reduce duplication
4. **Keep patch minimal** - avoid broad rewrites
5. **Provide verification notes** - explain what was changed and why

## Output Format

1. **Plan** (bullets)
2. **Unified diff patch**
3. **Notes** (rationale + risks)

## Checklist

- [ ] Public API unchanged
- [ ] I/O format unchanged
- [ ] Side effects unchanged
- [ ] No new dependencies
- [ ] Patch minimal and reversible

## Example

**Before:**
```python
def process_data(data):
    result = []
    for item in data:
        if item is not None:
            if item > 0:
                result.append(item * 2)
    return result
```

**After:**
```python
def process_data(data: list) -> list:
    return [item * 2 for item in data if item is not None and item > 0]
```

**Notes:**
- Simplified nested conditionals to list comprehension
- Added type hints for clarity
- External behavior unchanged: same input produces same output
