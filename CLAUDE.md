# CLAUDE.md

## Environment
- **OS:** Windows (PowerShell as default shell)
- **All scripts and commands must be PowerShell-compatible.** No bash, no `#!/bin/bash`, no `chmod`. Use `.ps1` scripts, `Invoke-WebRequest` not `curl`, `Remove-Item` not `rm`, etc.
- If a unix-only tool is required, note the Windows equivalent or flag it.

## Project
This is a Claude Code agentic template repository. It contains reusable
`.claude/` configurations, commands, agents, skills, and hooks. The distributable
template lives in `claude-code-template/`. Root-level files are working copies.

- **Stack**: Markdown templates, PowerShell hooks, JSON configuration
- **Key directories**: `.claude/` (working config), `claude-code-template/` (distributable)

## Stack
- Python 3.12+ (use `uv` for package management where available, fall back to `pip`)
- Testing: `pytest`
- Linting: `ruff check .`
- Formatting: `ruff format .`
- Type checking: `pyright` or `mypy`

## Commands
- `python -m pytest` — run tests
- `python -m pytest tests/test_specific.py -k "test_name"` — run single test (prefer this over full suite)
- `ruff check . --fix` — lint and autofix
- `ruff format .` — format
- `pyright .` — type check

## Workflow — IMPORTANT
1. **Plan first, code second.** For any non-trivial task, draft a plan and wait for approval before writing code. Use Plan Mode (Shift+Tab×2) or `/plan-and-review`.
2. **Verify your own work.** Never claim "done" until tests pass. Run `python -m pytest` after every meaningful change. If no tests exist for the changed logic, write them first.
3. **Small diffs only.** Do not refactor code that wasn't asked to be changed. Do not reorganize imports project-wide. Touch only what's needed.
4. **When corrected, update this file.** If I correct a mistake, ask: "Should I add this to CLAUDE.md so I don't repeat it?" Then do it.

## Conventions
- Conventional commits: `feat:`, `fix:`, `chore:`, `docs:`, `test:`, `refactor:`
- One logical change per commit
- Type hints on all function signatures
- Docstrings on public functions (Google style)
- Tests required for any changed logic — no exceptions

## Do Not
- Do not commit directly to `main`
- Do not add dependencies without flagging it first
- Do not use `os.system()` — use `subprocess.run()` with explicit args
- Do not delete or overwrite test files without confirmation
- Do not use unix-only paths (`/tmp/`, `/usr/`) — use `pathlib.Path` and `tempfile` for cross-platform compatibility
- Do not silently skip failing tests. If a test fails, fix it or explain why it should be changed.

## Cowork Tasks (Non-Coding)
When working on non-coding tasks (documents, research, organization):
- Still plan before executing. Outline deliverables, confirm structure, then produce.
- For multi-file outputs, list all files you'll create before starting.
- Use Markdown for documents unless a specific format is requested.
- When summarizing or researching, cite sources and flag uncertainty.

## Memory System
- **Rules**: `.claude/rules/memory.md` (sizing, anti-patterns), `.claude/rules/loop-resilience.md` (session resilience)
- **Commands**: `/checkpoint` (save state), `/memory-review` (audit), `/ralph-loop` (autonomous iteration)
- **Agent**: `loop-supervisor` (monitors ralph loops for stuck/gaming/drift)

## MCP Tools
- Notion MCP: Project documentation and task tracking
- Playwright MCP: Browser automation for verify-app agent

## Lessons Learned
- Template files in `claude-code-template/` must stay generic (use `[PLACEHOLDER]` markers)
- Root-level files are project-specific working copies -- keep them in sync manually
- `settings.local.json` should NOT be copied into the template (it contains per-machine grants)
- Keep CLAUDE.md under 100 lines; move domain details to `.claude/rules/` and `.claude/skills/`
