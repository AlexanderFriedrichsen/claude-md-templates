description: Save session state to MEMORY.md and progress files, then prepare for a clean restart.
argument-hint: "<optional: note about what to resume next>"

## Steps

1. **Gather state.** Review what was accomplished this session:
   - Read recent git diff or changed files to identify what was built/modified
   - Check if any tests are failing (quick test run)
   - Note any in-progress work or blockers

2. **Write structured progress.** Append a session entry to `.claude/plans/progress.md` (create if missing):
   ```
   ## Session [YYYY-MM-DD HH:MM]
   - **Status**: in-progress | blocked | completed
   - **Completed**: [bullet list of what was done]
   - **Next**: [numbered priority list — if human provided a resume note, that is #1]
   - **Blockers**: [any blocking issues, or "none"]
   - **Decisions**: [architectural decisions made this session]
   ```

3. **Update MEMORY.md.** Find the auto-memory directory for this project (it is under `~/.claude/projects/` with a path derived from the working directory). Write to `MEMORY.md` in that directory. Structure:
   - **Project Status**: one-line summary of current state
   - **What's Built**: bullet list of completed features/systems
   - **Immediate Next Steps**: numbered, most important first. If the human provided a resume note, make that #1.
   - **Key Architecture Decisions**: only add new ones, don't duplicate existing
   - **Conventions Reminder**: keep stable unless something changed

4. **Verify MEMORY.md is under 200 lines.** Lines after 200 are truncated from the system prompt. If approaching the limit, move detailed notes to topic-specific files (e.g., `memory/debugging.md`) and link from MEMORY.md.

5. **Checkpoint commit.** If there are uncommitted changes, create a checkpoint commit:
   `chore: checkpoint -- [one-line summary of current state]`
   This preserves work-in-progress in git history for session bridging.

6. **Report.** Tell the human:
   - What was saved (MEMORY.md + progress file + commit if created)
   - What to resume with next session (e.g., "say 'let's continue with [feature]' and I'll pick up from MEMORY.md")
   - Any failing tests or unfinished work to be aware of
