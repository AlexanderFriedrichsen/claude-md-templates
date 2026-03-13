description: Execute one iteration of an autonomous Ralph loop with progress tracking and circuit breakers.
argument-hint: "<optional: specific criterion to work on this iteration>"

## Overview

This command handles ONE iteration of a Ralph Wiggum loop. Progress lives in files, not memory.
An external orchestrator (script, stop hook, or manual re-invocation) calls this repeatedly.

## Steps

1. **Read spec.** Load `.claude/plans/ralph-spec.md`. If it does not exist, stop and tell the user:
   "Create `.claude/plans/ralph-spec.md` with: task description, acceptance criteria (numbered), and max iterations."

2. **Read progress.** Load `.claude/plans/ralph-progress.md`. If it does not exist, create it with
   `## Ralph Loop Progress` header and `Iteration: 0`. Parse the current iteration count.

3. **Check exit conditions** (before doing work):
   - If iteration count >= max iterations from spec: append `STATUS: STOP (max iterations)` and exit.
   - If the last 3 iteration entries show zero files changed: append `STATUS: STUCK (no progress)` and exit.
   - If the last 3 iteration entries show the same error: append `STATUS: STUCK (repeated error)` and exit.

4. **Identify work.** Compare acceptance criteria in spec against completed criteria in progress.
   Pick the next uncompleted criterion. If the human provided an argument, prioritize that.

5. **Execute.** Implement what is needed for the chosen criterion. Keep changes minimal and focused.

6. **Test.** Run the project's test command. Record tests passing and tests total.

7. **Commit.** Stage and commit: `chore(ralph): iteration N -- [brief summary]`

8. **Update progress.** Append an iteration entry to `.claude/plans/ralph-progress.md`:
   ```
   ### Iteration N
   - **Criterion**: [which acceptance criterion was targeted]
   - **Files changed**: [list from git diff --name-only HEAD~1]
   - **Tests**: [passing]/[total]
   - **Errors**: [any errors encountered, or "none"]
   - **Completed**: [list of criteria now done]
   STATUS: CONTINUE | DONE | STOP | STUCK
   ```
   The `STATUS:` line MUST be the final line of the entry (the orchestrator reads it).

9. **Report.** Print a one-line summary: `Iteration N: [STATUS] — [what was done]`

## Exit Statuses

- `DONE` — all acceptance criteria met
- `CONTINUE` — progress made, more work to do
- `STOP` — max iterations reached or human-requested stop
- `STUCK` — circuit breaker triggered

<!-- CUSTOMIZE: Replace with your shell. PowerShell example below, bash alternative in comments. -->

## Orchestrator Example (PowerShell)

```powershell
# ralph-runner.ps1
param([int]$MaxIterations = 20)
for ($i = 1; $i -le $MaxIterations; $i++) {
    Write-Host "=== Ralph iteration $i ===" -ForegroundColor Cyan
    claude --command ralph-loop
    $lastLine = Get-Content .claude/plans/ralph-progress.md | Select-Object -Last 1
    if ($lastLine -match "STATUS:\s*(DONE|STOP|STUCK)") {
        Write-Host "Loop ended: $($Matches[1])" -ForegroundColor Yellow
        break
    }
}
```

<!-- Bash alternative:
while true; do
  claude --command ralph-loop
  tail -1 .claude/plans/ralph-progress.md | grep -qE "STATUS: (DONE|STOP|STUCK)" && break
done
-->
