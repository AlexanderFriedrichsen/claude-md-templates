# Claude Code Agentic Template

A reusable `.claude/` configuration template for autonomous agentic development with Claude Code. Provides a structured plan-review-implement workflow, automated hooks, and skill-based domain knowledge.

## Quick Start

1. Copy the contents of this folder into your project root:
   ```
   cp -r claude-code-template/.claude /path/to/your-project/
   cp claude-code-template/CLAUDE.md.template /path/to/your-project/CLAUDE.md
   ```

2. Edit `CLAUDE.md` — fill in every `[PLACEHOLDER]` section with your project specifics.

3. Edit `.claude/rules/platform.md` — uncomment the section for your OS, delete the others.

4. Edit `.claude/rules/testing.md` — adjust the "What to Test" and "What NOT to Test" sections for your domain.

5. Edit `.claude/agents/staff-engineer.md` — add project-specific review criteria under the `[PROJECT-SPECIFIC]` markers.

6. Add domain skills in `.claude/skills/` — copy `_example-skill.md` and fill in your domain knowledge. Delete the example when done.

7. Review `.claude/settings.json` — adjust hooks for your linter, test runner, and protected paths.

## What's Included

### Core Workflow: `/plan-and-review`

The centerpiece. When invoked with a task description, it:

1. **Plans** — enters plan mode, explores the codebase, writes a detailed plan to `plans/current-plan.md`
2. **Reviews** — spawns the `staff-engineer` agent to critique the plan, writes findings to `plans/review.md`
3. **Iterates** — if FAIL, revises the plan and re-reviews (max 3 loops)
4. **Implements** — exits plan mode and builds the approved plan
5. **Verifies** — runs test suite, linter, type checker
6. **Recovers** — if verification fails, re-enters the plan/review loop with error context

This creates a self-correcting development loop that catches architectural issues before code is written.

### Commands (slash commands)

| Command | Purpose |
|---|---|
| `/plan-and-review <task>` | Full plan → review → implement → verify cycle |
| `/tdd <feature>` | Write failing tests first, then implement |
| `/commit-push` | Stage, commit with conventional message, push |
| `/checkpoint` | Save session state to MEMORY.md for cross-session continuity |
| `/research <topic>` | Research and produce a structured summary |
| `/codebase-index` | Generate file manifest for token-efficient navigation |

### Agents

| Agent | Purpose |
|---|---|
| `staff-engineer` | Reviews plans and code for soundness, edge cases, security |
| `code-simplifier` | Reduces code complexity post-implementation (optional) |
| `verify-app` | End-to-end verification beyond unit tests (optional) |

### Rules (always loaded)

| Rule | Purpose |
|---|---|
| `testing.md` | Testing philosophy, patterns, what to test/skip |
| `platform.md` | OS-specific shell, path, and tooling conventions |

### Skills (loaded on demand)

Skills are reference documents the agent consults when working in a specific domain. Add one per major domain area (e.g., `database.md`, `auth.md`, `api-design.md`).

Included skills:
- `quarto-qmd.md` — Quarto Markdown reference for `.qmd` files

### Hooks (automated guardrails)

| Hook | Trigger | Action |
|---|---|---|
| PostToolUse lint | After every file edit | Runs linter on changed file |
| Stop test | After task completion | Runs test suite |

### Plans directory

Working space for the plan/review cycle. `current-plan.md` holds the active plan, `review.md` holds the staff-engineer's verdict.

## Customization Guide

### Adding a new skill

1. Create `.claude/skills/your-domain.md`
2. Add YAML frontmatter with `name` and `description`
3. The `description` field tells Claude *when* to consult this skill — make it specific
4. Include data models, API references, domain rules, and common gotchas

### Adding a new command

1. Create `.claude/commands/your-command.md`
2. Add `description:` and `argument-hint:` at the top
3. Write numbered steps for the workflow
4. The command becomes available as `/your-command`

### Adding a new agent

1. Create `.claude/agents/your-agent.md`
2. Add YAML frontmatter with `name` and `description`
3. Write the system prompt and output format
4. Reference it in commands via `Spawn the your-agent subagent`

### Adjusting hooks

Edit `.claude/settings.json` under the `hooks` key. See `hooks/HOOKS_GUIDE.md` for the full reference and examples.

## File Structure

```
.claude/
├── agents/
│   ├── staff-engineer.md       # Code/plan reviewer
│   ├── code-simplifier.md      # Complexity reducer (optional)
│   └── verify-app.md           # E2E verification (optional)
├── commands/
│   ├── checkpoint.md           # Save session state
│   ├── codebase-index.md       # Generate file manifest
│   ├── commit-push.md          # Git commit + push
│   ├── plan-and-review.md      # Core agentic workflow
│   ├── research.md             # Research workflow
│   └── tdd.md                  # Test-driven development
├── hooks/
│   └── HOOKS_GUIDE.md          # Hook config (command + prompt hooks)
├── plans/
│   ├── .gitkeep
│   ├── current-plan.md         # Active plan (working file)
│   └── review.md               # Review verdict (working file)
├── rules/
│   ├── platform.md             # OS-specific conventions
│   └── testing.md              # Testing conventions
├── skills/
│   ├── _example-skill.md       # Skill template (delete after use)
│   └── quarto-qmd.md           # Quarto Markdown reference
├── settings.json               # Permissions, MCP servers, hooks
└── settings.local.json         # Local MCP permission grants
CLAUDE.md.template              # Project instructions (rename to CLAUDE.md)
```
