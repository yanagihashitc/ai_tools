# Commit only (current branch)

## Overview

This command is a simple template to commit local changes on the current branch.  
It does **not** handle pushing to the remote or enforcing branch strategies (such as disallowing direct commits to main); it only performs commits that follow the commit message rules.

## Preconditions

- Use the `/commit-message` skill to generate commit messages.

## Steps (non-interactive)

1. Review the uncommitted diff and decide on an appropriate commit message (e.g. using `git diff` or `git diff --cached`).
2. Stage changes (`git add -A`).
3. Commit (`git commit`) using an environment variable or argument for the message.

### A) Safe one-liner execution (with message argument)

```bash
MSG="<Prefix>: <summary (imperative / concise)>" \
git add -A && \
git commit -m "$MSG"
```

Example:

```bash
MSG="fix: remove unnecessary debug log output" \
git add -A && \
git commit -m "$MSG"
```

### B) Step-by-step execution (readability first)

```bash
# 1) Inspect changes
git status
git diff

# 2) Stage changes
git add -A

# 3) Commit (edit the message inline)
git commit -m "<Prefix>: <summary (imperative / concise)>"
```

## Notes

- Branch strategy (e.g. disallowing direct commits to main) and pushing to the remote (`git push`) are **out of scope** for this command.


