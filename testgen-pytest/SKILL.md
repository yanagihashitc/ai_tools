---
name: testgen-pytest
description: Generates minimal, high-value pytest tests focusing on black-box testing against public API. Creates 3-8 meaningful tests without overfitting to implementation details.
metadata:
  author: Team
  version: "1.0"
---

# Pytest Test Generation

You generate MINIMAL, HIGH-VALUE pytest tests that verify behavior without overfitting to implementation.

## When to Use This Skill

- After refactoring code to ensure behavior is preserved
- When adding test coverage to existing code
- When validating edge cases and error handling

## Hard Rules

- Prefer black-box tests against public API
- Do not overfit to implementation details
- Keep tests few but meaningful (3-8)
- No new dependencies

## Steps

1. **Read the refactor diff (or target code)** and identify risk points:
   - Boundary conditions
   - Error handling
   - Data shape transforms

2. **Write tests asserting invariants:**
   - Same output for same input
   - Errors thrown consistently

3. **Keep setup light** - use fixtures only if it reduces duplication

4. **Avoid mocking** unless required by external I/O

## Output Format

1. **Test Plan** (short)
2. **Unified diff patch** (new/updated test files)
3. **Notes** (assumptions)

## Checklist

- [ ] 3-8 tests
- [ ] High-value invariants/edges covered
- [ ] Minimal scaffolding
- [ ] pytest runnable

## Example

**Target function:**
```python
def calculate_discount(price: float, discount_percent: float) -> float:
    if discount_percent < 0 or discount_percent > 100:
        raise ValueError("Invalid discount")
    return price * (1 - discount_percent / 100)
```

**Generated tests:**
```python
import pytest
from module import calculate_discount

def test_zero_discount():
    assert calculate_discount(100, 0) == 100

def test_full_discount():
    assert calculate_discount(100, 100) == 0

def test_partial_discount():
    assert calculate_discount(100, 25) == 75

def test_invalid_negative_discount():
    with pytest.raises(ValueError):
        calculate_discount(100, -10)

def test_invalid_over_100_discount():
    with pytest.raises(ValueError):
        calculate_discount(100, 150)
```
