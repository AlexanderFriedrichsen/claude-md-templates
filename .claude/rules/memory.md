# Memory Management Rules

## Memory Hierarchy (highest precedence last)

1. **Managed policy** — `C:\Program Files\ClaudeCode\CLAUDE.md` (org-wide, IT-deployed)
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

When writing to MEMORY.md, allocate lines per section:

- **Project Status**: 5 lines (one-line summary + key metrics)
- **Architecture**: 25 lines (stack, data flow, key patterns)
- **Decisions**: 25 lines (ADRs — date-stamped, one line each)
- **Patterns**: 25 lines (recurring solutions, conventions confirmed across sessions)
- **Gotchas**: 20 lines (hard-won lessons, things that break)
- **Progress**: 30 lines (current state, next steps — volatile, update often)
- **Buffer**: 70 lines (overflow / new sections as needed)

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
- If a memory entry contradicts current code, the code is the source of truth — update the memory

### Memory Conflicts
- If two files give contradictory instructions, the more-specific file wins
- Never have the same rule in both CLAUDE.md and a rules file
- When updating a convention, search all memory tiers for the old version

## Compounding Engineering

When corrected on a mistake, always ask: **"Should I add this to CLAUDE.md so I don't repeat it?"**

Every correction added to memory pays dividends permanently. The cost of documenting a lesson is minutes; the cost of repeating a mistake is hours.
