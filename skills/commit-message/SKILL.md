---
name: commit-message
description: Generate Git commit messages following Conventional Commits format. Use when creating commit messages, committing changes, or when the user asks for help writing commit messages. Triggers on commit-related tasks including git commit, creating commits, or drafting commit messages.
---

# Commit Message Skill

Generate commit messages based on Conventional Commits with bullet-list bodies.

## Format

```
<Prefix>: <summary (imperative / concise, ~50 chars)>

- Change item 1
- Change item 2
- ...

Refs: #<issue-number> (optional)
BREAKING CHANGE: <description> (optional)
```

## Prefix Types

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
| style | Style-only changes (no logic impact) |
| revert | Revert a previous change |

Optional scope: `<Prefix>(scope): ...` (e.g., `fix(auth): ...`)

## Workflow

1. **Inspect the diff**: Always run `git diff` or `git diff --cached` to see actual changes
2. **Determine prefix**: Choose the appropriate prefix based on change type
3. **Write summary**: Concise imperative summary (~50 chars, no trailing period)
4. **List changes**: Bullet points starting with `- `
5. **Add footer**: Reference issues with `Refs: #123` or `Closes: #123`

## Language

- Summary and body: English (`language = "en"`)
- Technical terms can remain in English regardless of language setting

## Examples

```
fix: remove unnecessary debug log output

- Remove redundant log lines from user information fetch processing
- Reduce log volume while keeping necessary information

Refs: #123
```

```
refactor: extract duplicated validation logic into common function

- Extract duplicate form input validation code into a utility function
- Remove duplicated logic at call sites to improve readability
- No behavioral changes
```

## Prohibited Patterns

- Vague summaries: "update", "fix bug", "changes"
- Non-English summaries (when `language = "en"`)
- Long bodies without bullet lists
- Commits that only weaken static analysis without justification
