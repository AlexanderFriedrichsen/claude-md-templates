description: Generate a file manifest and codebase map for token-efficient navigation.
argument-hint: "<optional: specific directory to index>"

## Purpose

Creates a compact codebase index that helps Claude navigate the repo without
reading every file. The index is stored in `.claude/codebase-index.md` and
should be regenerated when the project structure changes significantly.

## Steps

1. **Scan.** Walk the project directory tree, excluding: `node_modules`, `.git`,
   `__pycache__`, `dist`, `build`, `.next`, `.venv`, `venv`, `.claude/plans`.

2. **Classify.** For each file, record:
   - Path (relative to project root)
   - Purpose (inferred from name, location, and first 5 lines)
   - Key exports or entry points (for source files)

3. **Structure.** Write the index to `.claude/codebase-index.md`:

   ```markdown
   # Codebase Index
   Generated: [date]

   ## Directory Map
   [Tree structure with 1-line descriptions per directory]

   ## Key Files
   | File | Purpose | Key Exports |
   |------|---------|-------------|
   | src/index.ts | Entry point | startServer() |
   | ... | ... | ... |

   ## Architecture Notes
   [2-3 sentences on data flow, key patterns, dependency direction]

   ## Entry Points
   - Main: [file]
   - Tests: [command]
   - Config: [files]
   ```

4. **Verify.** Confirm the index is under 150 lines. If over, summarize
   subdirectories instead of listing individual files.

5. **Report.** Tell the human the index was generated and where to find it.

## When to Regenerate

- After adding new directories or major files
- After significant refactoring
- At the start of a new development sprint
- When Claude seems confused about project structure
