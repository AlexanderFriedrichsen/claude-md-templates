# Hooks Configuration

Hooks are configured in `.claude/settings.json` (create this file if it doesn't exist). They run deterministically — unlike CLAUDE.md instructions, they cannot be ignored.

## Hook Types

| Event | When it fires |
|---|---|
| `PreToolUse` | Before a tool executes (can block) |
| `PostToolUse` | After a tool executes |
| `Stop` | When the agent finishes a task |
| `PreCompact` | Before context window compaction (forward-looking — see notes below) |

## Matcher

Use `matcher` to filter which tools trigger the hook. Supports `|` for OR:
- `"Edit|Write"` — fires on file edits and writes
- `"Bash"` — fires on shell commands

## Failure Modes

- `"block"` — prevents the action from proceeding. Use for non-negotiable checks.
- `"warn"` — shows the output but lets the agent continue. Use for advisory checks.

## Example: Lint after file edit

<!-- CUSTOMIZE: Uncomment the variant for your OS and stack -->

### Unix (Node/TypeScript)

```json
{
  "PostToolUse": [
    {
      "matcher": "Edit|Write",
      "hooks": [
        {
          "type": "command",
          "command": "npx eslint $CLAUDE_FILE_PATH --quiet 2>/dev/null || true",
          "description": "Lint changed file after edit"
        }
      ]
    }
  ]
}
```

### Windows (PowerShell) variant

```json
{
  "command": "powershell -Command \"$file = $env:CLAUDE_FILE_PATH; if ($file -match '\\.(ts|tsx|js|jsx)$') { npx eslint $file --quiet 2>&1 } else { exit 0 }\"",
  "description": "Lint changed JS/TS file after edit"
}
```

### Python (ruff)

```json
{
  "command": "ruff check $CLAUDE_FILE_PATH --quiet 2>/dev/null || true",
  "description": "Lint changed Python file after edit"
}
```

## Example: Run tests on task completion

### Unix

```json
{
  "Stop": [
    {
      "hooks": [
        {
          "type": "command",
          "command": "pnpm test --if-present 2>&1 | tail -5",
          "description": "Run tests after task completion"
        }
      ]
    }
  ]
}
```

### Windows variant

```json
{
  "command": "powershell -Command \"pnpm test --if-present 2>&1 | Select-Object -Last 5\"",
  "description": "Run tests after task completion"
}
```

## Example: Block writes to protected paths

### Unix

```json
{
  "PreToolUse": [
    {
      "matcher": "Edit|Write",
      "hooks": [
        {
          "type": "command",
          "command": "echo $CLAUDE_FILE_PATH | grep -qE '(migrations|.env)' && exit 1 || exit 0",
          "description": "Block edits to migrations and env files"
        }
      ]
    }
  ]
}
```

### Windows variant

```json
{
  "command": "powershell -Command \"$file = $env:CLAUDE_FILE_PATH; if ($file -match '(migrations|.env)') { exit 1 }\"",
  "description": "Block edits to migrations and env files"
}
```

## Notes

- Hooks receive `$CLAUDE_FILE_PATH` as an environment variable with the path of the affected file.
- On Windows, use `powershell -Command "..."` or call a `.ps1` script for complex logic.
- Keep hook commands fast (<2 seconds). Slow hooks degrade the development loop.
- The `--if-present` flag on test commands prevents errors when no test runner is configured yet.

## Prompt Hooks (LLM-Based Evaluation)

Prompt hooks use an LLM to evaluate tool inputs/outputs rather than running a
shell command. They are slower but can assess semantic quality.

### Example: Prevent overly broad file writes

```json
{
  "PreToolUse": [
    {
      "matcher": "Write",
      "hooks": [
        {
          "type": "prompt",
          "prompt": "Review the file content about to be written. REJECT if it: (1) contains hardcoded secrets or API keys, (2) overwrites an entire file when only a small edit was needed, (3) removes existing test cases. Otherwise ALLOW.",
          "description": "LLM guard: check file writes for common mistakes"
        }
      ]
    }
  ]
}
```

### Notes on Prompt Hooks

- Prompt hooks are evaluated by a fast model, not the main session model.
- They add latency (~1-3 seconds per evaluation). Use sparingly.
- The hook receives the full tool input as context.
- Return `ALLOW` or `REJECT` with a reason. Any other response is treated as ALLOW.
- Combine with command hooks: use command hooks for fast/deterministic checks,
  prompt hooks for semantic/judgment-based checks.

## PreCompact Hook (Forward-Looking)

The `PreCompact` hook fires before Claude Code compresses the context window.
This is your last chance to capture session state before older messages are
summarized and potentially lost.

**Key constraint**: The context window is already at capacity when this fires,
so the hook must be lightweight — a shell command, not an LLM evaluation.

### Unix

```json
{
  "PreCompact": [
    {
      "hooks": [
        {
          "type": "command",
          "command": "echo \"### Compaction $(date '+%Y-%m-%d %H:%M')\" >> .claude/plans/progress.md",
          "description": "Log compaction event to progress file"
        }
      ]
    }
  ]
}
```

### Windows variant

```json
{
  "command": "powershell -File .claude/hooks/log-compaction.ps1",
  "description": "Log compaction event to progress file"
}
```

Create `.claude/hooks/log-compaction.ps1`:

```powershell
$timestamp = Get-Date -Format 'yyyy-MM-dd HH:mm'
$entry = "`n### Compaction $timestamp`nContext compacted. Re-read plan and progress files."
Add-Content -Path .claude/plans/progress.md -Value $entry
```

### Compatibility Note

PreCompact is a newer Claude Code hook event. If your version does not support it,
the hook entry in `.claude/settings.json` is silently ignored — no errors, no side effects.
