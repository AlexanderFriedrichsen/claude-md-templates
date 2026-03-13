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

3. **Update MEMORY.md.** Find the auto-memory directory for this project (under `~/.claude/projects/<project>/memory/`). This is a Claude Code system directory outside your repo — not committed to git. Write to `MEMORY.md`:
   - **Project Status**: one-line summary of current state
   - **What's Built**: bullet list of completed features/systems
   - **Immediate Next Steps**: numbered, most important first
   - **Key Architecture Decisions**: only add new ones, don't duplicate existing
   - **Conventions Reminder**: keep stable unless something changed

4. **Verify MEMORY.md is under 200 lines.** If approaching the limit, move detailed notes to topic-specific files and link from MEMORY.md.

5. **Checkpoint commit.** If there are uncommitted changes, create a checkpoint commit:
   `chore: checkpoint -- [one-line summary of current state]`

6. **Report.** Tell the human:
   - What was saved (MEMORY.md + progress file + commit if created)
   - What to resume with next session
   - Any failing tests or unfinished work to be aware of
