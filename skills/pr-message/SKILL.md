---
name: pr-message
description: Generate Pull Request titles and bodies following Conventional Commits format with structured sections. Use when creating PRs, drafting PR descriptions, or when the user asks for help writing PR messages. Triggers on PR-related tasks including gh pr create, creating pull requests, or drafting PR content.
---

# PR Message Skill

Generate PR messages with Conventional Commits titles and structured bodies.

## Title Format

```
<Prefix>: <summary (imperative / concise, ~50 chars)>
```

### Prefix Types

| Prefix | Use Case |
|--------|----------|
| feat | New feature |
| fix | Bug fix |
| refactor | Refactoring (no behavior change) |
| perf | Performance improvement |
| test | Tests added/updated |
| docs | Documentation updates |
| build | Build/dependency changes |
| ci | CI-related changes |
| chore | Chores (tools, scripts, etc.) |

## Body Template

```markdown
## Overview

Short summary of what this PR implements or fixes.

## Changes

- Describe change 1
- Describe change 2
- Describe change 3

## Technical details (optional)

- Describe implementation details or design intent, if needed

## Tests

- Types of tests run (unit tests, E2E tests, manual verification, etc.)
- Key verification results

## Related issues

- Closes #123
- Refs #456
```

**Required sections:** Overview, Changes  
**Optional sections:** Technical details, Tests, Related issues

## Workflow

1. **Inspect the diff**: Run `git diff [base-branch]...HEAD` and `git log` to see all changes
2. **Determine prefix**: Choose based on the primary change type
3. **Write title**: Concise imperative summary (~50 chars, no trailing period)
4. **Write Overview**: 1-2 sentences describing what the PR does
5. **List Changes**: Bullet points for each significant change
6. **Add Tests section**: Describe verification performed
7. **Link issues**: Reference related issues with `Closes` or `Refs`

## Language

- Title and body: English (`language = "en"`)
- Technical terms can remain in English regardless of language setting

## Example

**Title:**
```
feat: add user authentication with JWT tokens
```

**Body:**
```markdown
## Overview

Implement JWT-based authentication for the API endpoints.

## Changes

- Add JWT token generation and validation in auth module
- Create login and refresh token endpoints
- Add authentication middleware for protected routes
- Update user model with password hashing

## Technical details

- Using RS256 algorithm for token signing
- Access tokens expire in 15 minutes, refresh tokens in 7 days

## Tests

- Unit tests for token generation/validation
- Integration tests for auth endpoints
- Manual verification of login flow

## Related issues

- Closes #45
- Refs #12
```

## Prohibited Patterns

- Vague titles: "update", "fix issue", "changes"
- Non-English content (when `language = "en"`)
- Unstructured walls of text without headings
- Descriptions that contradict or omit actual changes
