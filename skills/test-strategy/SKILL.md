---
name: test-strategy
description: Guide test implementation with equivalence partitioning, boundary value analysis, and structured test cases. Use when creating tests, writing test code, modifying existing tests, implementing unit tests, integration tests, or E2E tests. Triggers on test-related tasks including adding tests, updating tests, or when asked to ensure test coverage.
---

# Test Strategy Skill

Implement tests following equivalence partitioning, boundary value analysis, and structured documentation.

## Workflow

1. **Present test perspectives table** (before writing any test code)
2. **Implement all test cases** from the table
3. **Add Given/When/Then comments** to each test
4. **Verify exception handling** with type and message assertions
5. **Document test commands and coverage**

## Step 1: Test Perspectives Table

Present a Markdown table before writing tests:

| Case ID | Input / Precondition | Perspective (Equivalence / Boundary) | Expected Result | Notes |
|---------|---------------------|--------------------------------------|-----------------|-------|
| TC-N-01 | Valid input A | Equivalence – normal | Success with expected value | - |
| TC-A-01 | NULL | Boundary – NULL | Validation error | - |
| TC-A-02 | Empty string | Boundary – empty | Validation error | - |
| TC-B-01 | Minimum value | Boundary – min | Success at boundary | - |
| TC-B-02 | Maximum value | Boundary – max | Success at boundary | - |
| TC-B-03 | Min - 1 | Boundary – below min | Validation error | - |
| TC-B-04 | Max + 1 | Boundary – above max | Validation error | - |

**Required boundary values:** 0, min, max, ±1, empty, NULL  
**Skip rule:** Omit boundaries not meaningful for the spec, document reason in Notes

## Step 2: Test Implementation Policy

- Implement **all cases** from the perspectives table
- Include **equal or more failure cases** than success cases
- Cover: normal paths, validation errors, exceptions, boundary values, invalid types, external dependency failures
- Target **100% branch coverage** (document uncovered branches with reasons)

## Step 3: Given/When/Then Comments

Add to every test case:

```
// Given: Preconditions
// When: Action performed
// Then: Expected result/assertion
```

## Step 4: Exception Verification

- Assert exception **type** and **message**
- Verify error codes and field info for validation errors
- Use mocks/stubs to simulate external dependency failures

## Step 5: Document Commands and Coverage

End test implementation with:

```markdown
## Test Execution

- Command: `pytest --cov=module_name` / `pnpm vitest run --coverage`
- Coverage target: 100% branch coverage
```

## Minor Test Changes

For minor edits (message adjustments, small expected value changes) without new branches, the perspectives table update is optional.
