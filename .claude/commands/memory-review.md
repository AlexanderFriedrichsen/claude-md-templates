description: Audit memory files for bloat, duplicates, and conflicts, then propose consolidation.
argument-hint: "<optional: focus area — e.g. 'MEMORY.md only' or 'check for duplicates'>"

## Steps

1. **Locate memory files.** Find and read all memory sources:
   - `MEMORY.md` in the auto-memory directory (`~/.claude/projects/<project>/memory/`)
   - Any topic files in that memory directory (e.g., `debugging.md`, `patterns.md`)
   - Root `CLAUDE.md`
   - All files in `.claude/rules/`
   - `CLAUDE.local.md` (if it exists)

2. **Audit line counts.** Check against budgets:
   - `MEMORY.md` > 200 lines → flag (only first 200 auto-load)
   - `CLAUDE.md` > 100 lines → flag
   - Any rule file > 80 lines → flag
   Report current line counts for all files.

3. **Check for duplicates.** Scan for:
   - Same instruction appearing in multiple files (e.g., same rule in CLAUDE.md and a rules file)
   - Near-duplicate entries within a single file (same concept phrased differently)

4. **Check for conflicts.** Look for contradictory instructions across files:
   - One file says "use X" while another says "use Y" for the same concern
   - Outdated conventions that conflict with newer rules

5. **Propose changes.** For each issue found, propose a specific action:
   - **Delete**: entry is stale, duplicated, or no longer relevant
   - **Merge**: two entries cover the same topic — combine into one
   - **Move**: entry is in the wrong tier (e.g., volatile progress in CLAUDE.md should be in MEMORY.md)
   - **Trim**: entry is too verbose — suggest a shorter version

6. **Confirm.** Present the full list of proposed changes to the human. **Do not make changes without explicit approval.**

7. **Apply.** If approved, make the changes. Re-check line counts after.

8. **Report.** Final summary:
   - Files audited (with before/after line counts)
   - Changes made
   - Remaining issues (if any were declined)

## Notes

- This is a v1 audit — it checks structure (line counts, duplicates, conflicts) but does NOT cross-reference memory entries against the codebase to detect staleness. That is a planned v2 enhancement.
- If the human specified a focus area, narrow the audit to that scope.
- When in doubt about whether an entry is stale, keep it and flag it for human review.
