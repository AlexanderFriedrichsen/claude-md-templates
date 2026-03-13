description: Audit memory files for bloat, duplicates, and conflicts, then propose consolidation.
argument-hint: "<optional: focus area — e.g. 'MEMORY.md only' or 'check for duplicates'>"

## Steps

1. **Locate memory files.** Find and read all memory sources:
   - `MEMORY.md` in the auto-memory directory (`~/.claude/projects/<project>/memory/`).
     Note: This is a Claude Code system directory outside your repo — not committed to git.
   - Any topic files in that memory directory
   - Root `CLAUDE.md`
   - All files in `.claude/rules/`
   - `CLAUDE.local.md` (if it exists)

2. **Audit line counts.** Check against budgets:
   - `MEMORY.md` > 200 lines → flag
   - `CLAUDE.md` > 100 lines → flag
   - Any rule file > 80 lines → flag
   Report current line counts for all files.

3. **Check for duplicates.** Scan for:
   - Same instruction appearing in multiple files
   - Near-duplicate entries within a single file

4. **Check for conflicts.** Look for contradictory instructions across files.

5. **Propose changes.** For each issue found, propose a specific action:
   - **Delete**: stale, duplicated, or no longer relevant
   - **Merge**: two entries cover the same topic
   - **Move**: entry is in the wrong tier
   - **Trim**: entry is too verbose

6. **Confirm.** Present all proposed changes. **Do not make changes without explicit approval.**

7. **Apply.** If approved, make the changes. Re-check line counts after.

8. **Report.** Final summary with before/after line counts and changes made.
