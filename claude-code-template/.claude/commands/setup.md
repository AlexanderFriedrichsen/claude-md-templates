description: Set up the Claude Code agentic template in a new project directory. Walks through copying files, customizing CLAUDE.md, and configuring rules for the target project.
argument-hint: "<target directory path>"

## Steps

1. **Validate the target.** The user provided a target directory as the argument: `$ARGUMENTS`. If no argument was given, ask the user for the path to their project directory. Verify the directory exists. Check whether a `.claude/` directory already exists there — if so, warn the user and ask whether to merge (keep existing files, only add missing ones) or overwrite.

2. **Copy the template.** Copy the contents of the template's `.claude/` directory into the target project's `.claude/` directory. Also copy `CLAUDE.md.template` to the target as `CLAUDE.md` (unless one already exists — if so, ask before overwriting). Do NOT copy this `setup.md` command itself — the setup command belongs in the template repo, not in every project.

3. **Gather project details.** Ask the user about their project so you can fill in CLAUDE.md. Collect:
   - Project name and one-sentence description
   - Stack (language, framework, database, package manager)
   - Key directories (e.g., `src/`, `lib/`, `app/`)
   - Test command, lint command, format command, type-check command, build command
   - Any project-specific constraints (e.g., "no new dependencies without approval", "Python 3.12+")
   - OS preference (Windows/macOS/Linux) for platform.md rules

4. **Fill in CLAUDE.md.** Open the copied `CLAUDE.md` in the target directory and replace all `[PLACEHOLDER]` markers and template sections with the user's answers from step 3. Remove the HTML guide comments (`<!-- ... -->`) once filled in — they're scaffolding, not permanent content. Keep the file under 100 lines.

5. **Configure platform.md.** Based on the user's OS preference, populate `.claude/rules/platform.md` with appropriate conventions:
   - **Windows**: PowerShell syntax, no bash-isms, use `pathlib.Path`, no unix paths
   - **macOS/Linux**: Bash syntax, unix conventions
   - **Cross-platform**: Use `pathlib.Path`, `tempfile`, avoid OS-specific assumptions

6. **Configure testing.md.** Update `.claude/rules/testing.md`:
   - Fill in the `## Tools` section with the project's test framework (pytest, vitest, jest, etc.)
   - Adjust the `## Structure` section for the project's test file naming convention
   - Keep the testing patterns section relevant — remove framework-specific patterns that don't apply (e.g., remove the Vitest section if using pytest, and vice versa)

7. **Customize agents.** In `.claude/agents/staff-engineer.md`, replace the `[PROJECT-SPECIFIC]` placeholder sections with review criteria relevant to the user's stack. Ask the user if they have specific review criteria to add (e.g., "always check for SQL injection", "verify RLS policies"). If they don't, fill in sensible defaults for their stack.

8. **Clean up example files.** Delete `.claude/skills/_example-skill.md` from the target. Ask the user if they want to create their first domain skill now. If yes, ask for the domain name and key facts, then create a skill file from the example template structure. If no, skip.

9. **Set up hooks (optional).** Ask the user if they want to configure hooks now. If yes:
   - Ask which linter/formatter they use
   - Create a `.claude/settings.json` with a PostToolUse hook that runs the linter on edited files
   - Reference `.claude/hooks/HOOKS_GUIDE.md` for further customization
   If no, skip — they can do it later using the hooks guide.

10. **Set up auto-memory.** Explain to the user that Claude Code maintains a per-project `MEMORY.md` at `~/.claude/projects/<project>/memory/MEMORY.md` (outside the repo, not committed to git). This is created automatically by Claude Code. Show them the structure from `MEMORY.md.template` so they know what to expect. Do NOT copy `MEMORY.md.template` into the target project — it's a reference only.

11. **Initialize git tracking.** Check if the target directory is a git repo. If so, suggest staging the new `.claude/` directory and `CLAUDE.md`. Remind the user to add `CLAUDE.local.md` to `.gitignore` (it's for personal, non-shared config). If not a git repo, just note that `.claude/` should be committed when they initialize one.

12. **Report.** Summarize what was set up:
    - List all files created/modified in the target directory
    - Highlight what still needs manual attention (e.g., "Add domain skills as you learn your codebase")
    - Note where auto-memory will live (`~/.claude/projects/<project>/memory/MEMORY.md`)
    - Suggest next steps: "Run `/plan-and-review <your first task>` to try the workflow"
