# Memory Management Rules

## Memory Hierarchy (highest precedence last)

1. **Managed policy** — org-wide CLAUDE.md (IT-deployed)
2. **User memory** — `~/.claude/CLAUDE.md` (personal, all projects)
3. **Project memory** — `./CLAUDE.md` or `./.claude/CLAUDE.md` (team-shared via git)
4. **Project rules** — `./.claude/rules/*.md` (modular, always-loaded)
5. **Local memory** — `./CLAUDE.local.md` (personal, one project, gitignored)
6. **Auto memory** — `~/.claude/projects/<project>/memory/MEMORY.md` (Claude-authored)

More-specific files override less-specific. Never duplicate info across tiers.

## Sizing Budgets

| File | Max Lines | Reason |
|------|-----------|--------|
| `MEMORY.md` | 200 | Only first 200 lines auto-load into system prompt |
| `CLAUDE.md` | 100 | Keep concise; move details to rules/skills |
| Rule files | 80 | Loaded every session; respect context budget |
| Topic files (`memory/*.md`) | Unlimited | Loaded on-demand only |

When approaching a limit, move detailed content to topic-specific files and link from the parent.

## What to Store Where

| Memory Type | Where | Examples |
|-------------|-------|---------|
| **Semantic** (facts, conventions) | CLAUDE.md, `.claude/rules/` | Stack choices, coding standards, API patterns |
| **Episodic** (what happened) | Git commits, `.claude/plans/progress.md` | Session logs, iteration outcomes, decisions |
| **Procedural** (how to do things) | `.claude/skills/`, `.claude/commands/` | Workflows, domain recipes, tool usage |
| **Working** (current task) | Context window | Current file contents, conversation state |

## MEMORY.md Section Budgets

<!-- CUSTOMIZE: Adjust line budgets per section based on your project's needs -->

- **Project Status**: 5 lines
- **Architecture**: 25 lines
- **Decisions**: 25 lines
- **Patterns**: 25 lines
- **Gotchas**: 20 lines
- **Progress**: 30 lines
- **Buffer**: 70 lines

## Anti-Patterns

### Memory Bloat
- Do not store session-specific context (temp state, in-progress debugging)
- Do not duplicate info that exists in CLAUDE.md or rules
- Consolidate similar entries rather than appending new ones
- Delete entries for features/code that no longer exists

### Stale Memory
- Date-stamp volatile entries (decisions, progress)
- Review memory quarterly or when the project structure changes significantly
- Use `/memory-review` to audit for staleness
- If a memory entry contradicts current code, the code is the source of truth

### Memory Conflicts
- If two files give contradictory instructions, the more-specific file wins
- Never have the same rule in both CLAUDE.md and a rules file
- When updating a convention, search all memory tiers for the old version

## Compounding Engineering

When corrected on a mistake, always ask: **"Should I add this to CLAUDE.md so I don't repeat it?"**
