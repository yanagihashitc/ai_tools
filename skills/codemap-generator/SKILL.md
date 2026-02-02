---
name: codemap-generator
description: Generate architectural codemaps from codebase structure. Use when creating or updating documentation maps of a codebase, analyzing repository structure, mapping module dependencies, or generating docs/CODEMAPS/* files. Triggers on requests like "create codemap", "update architecture docs", "map the codebase", or "generate project structure documentation".
---

# Codemap Generator

Generate architectural codemaps that document codebase structure, module relationships, and data flows.

## Quick Start

1. Analyze repository structure
2. Map modules and dependencies
3. Generate codemaps to `docs/CODEMAPS/`

## Workflow

### Step 1: Repository Analysis

```
a) Identify workspaces/packages (apps/*, packages/*, services/*)
b) Map directory structure
c) Find entry points (layout.tsx, index.ts, main.py)
d) Detect framework patterns (Next.js, FastAPI, etc.)
```

### Step 2: Module Analysis

For each module:
- Extract exports (public API)
- Map imports (dependencies)
- Identify routes (API routes, pages)
- Find database models
- Locate workers/background jobs

### Step 3: Generate Codemaps

Output structure:
```
docs/CODEMAPS/
├── INDEX.md              # Overview of all areas
├── frontend.md           # Frontend structure
├── backend.md            # Backend/API structure
├── database.md           # Database schema
├── integrations.md       # External services
└── workers.md            # Background jobs
```

## Codemap Format

Use this template for each codemap:

```markdown
# [Area] Codemap

**Last Updated:** YYYY-MM-DD
**Entry Points:** list main files

## Architecture

[ASCII diagram of component relationships]

## Key Modules

| Module | Purpose | Exports | Dependencies |
|--------|---------|---------|--------------|
| ... | ... | ... | ... |

## Data Flow

[Description of data flow through this area]

## External Dependencies

- package-name - Purpose, Version

## Related Areas

Links to other codemaps
```

## Analysis Tools

Use these commands when available:

```bash
# TypeScript/JavaScript projects (requires ts-morph)
npx tsx scripts/codemaps/generate.ts

# Dependency graph visualization (requires madge)
npx madge --image graph.svg src/

# JSDoc extraction (requires jsdoc2md)
npx jsdoc2md src/**/*.ts
```

## Templates

See [references/templates.md](references/templates.md) for complete codemap examples:
- Frontend codemap template
- Backend codemap template
- Integrations codemap template
- INDEX.md template

## Best Practices

1. **Generate from code** - Don't manually write; extract from actual structure
2. **Freshness timestamps** - Always include last updated date
3. **Under 500 lines** - Keep each codemap concise
4. **Consistent format** - Use same markdown structure across files
5. **Cross-reference** - Link related codemaps
6. **Verify paths** - Ensure all file paths exist

## Quality Checklist

Before committing:
- [ ] Generated from actual code structure
- [ ] All file paths verified to exist
- [ ] Timestamps updated
- [ ] ASCII diagrams are clear
- [ ] No obsolete references
