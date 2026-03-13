# Loop Resilience Rules

Rules for surviving long sessions, autonomous loops, and context degradation.

## Context Window Management

- Use `/compact` proactively when context is getting heavy (~70% capacity).
- Use `/clear` when switching to an unrelated task.
- After compaction, re-read the plan file to restore critical context.

## Fresh Context Pattern

For tasks spanning multiple sessions or autonomous loops:
- **Progress lives in files, not memory.** Write state to `.claude/plans/progress.md` and git commits.
- **Each session starts by reading state from disk.** Check git log, progress files, and the plan.
- Prefer restarting with a plan file over continuing in a degraded context window.

## Session Bridging

Before ending any session:
1. Commit all work-in-progress
2. Update `.claude/plans/progress.md` with current status, next steps, and blockers
3. Run `/checkpoint` to save memory state

When starting a new session:
1. Read `.claude/plans/progress.md` and recent git log
2. Read the active plan if one exists
3. Run tests to verify current state before making changes

## Circuit Breakers

Stop and reassess if:
- **Same fix attempted 3 times** — the approach is wrong. Re-plan.
- **No file changes in 3 consecutive turns** — act or ask the human.
- **Same error appears 3 times** — the error is systemic. Step back.
- **Tests deleted to make suite pass** — this is metric gaming. Revert and fix.

When a circuit breaker triggers:
1. Stop current work
2. Document what was tried and why it failed in the progress file
3. Re-enter plan mode or ask the human for guidance

## Anti-Drift

During long implementation sessions:
- Re-read the plan every ~10 tool calls to verify you are still on track.
- If current work diverges from the plan, stop and reconcile.
- Do not add features, refactor unrelated code, or "improve" things not in the plan.

## File Change Tracking

Use `git diff --name-only` to concretely track what changed per iteration. Do not rely on memory of what you edited — check the diff.

## Ralph Loop Safety

When operating in a Ralph loop (`/ralph-loop`):
- Each invocation is one iteration. Do not try to complete everything in one pass.
- Read the spec and progress file at the start of every iteration.
- Write a progress entry at the end of every iteration, always ending with a `STATUS:` line.
- If the loop supervisor says STOP, stop.
