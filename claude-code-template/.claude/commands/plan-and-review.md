description: Draft a plan, get staff engineer review, loop until approved, then implement with verification.
argument-hint: "<task description>"

## Workflow

1. **Plan phase.** Enter plan mode. If `.claude/codebase-index.md` exists, read it first for project orientation. Then explore specific areas as needed. Write a detailed implementation plan to `.claude/plans/current-plan.md`. The plan must include:
   - Goal (1 sentence)
   - Files to create/modify (explicit list)
   - Step-by-step implementation approach
   - Testable acceptance criteria
   - Risks or open questions

2. **Review phase.** Spawn the `staff-engineer` subagent to review `.claude/plans/current-plan.md`.

3. **Evaluate.** Read `.claude/plans/review.md`.
   - If VERDICT is **FAIL**: incorporate feedback, revise plan, return to step 2. Max 3 revision loops.
   - If VERDICT is **PASS** or **PASS WITH NOTES**: proceed to step 4.
   - If QUESTIONS_FOR_HUMAN is non-empty: pause and ask the human. Resume after answer.

4. **Implement.** Exit plan mode. Implement the approved plan. Keep diffs small and focused. Commit after each logical unit of work.

5. **Verify.** Run the full verification suite (test, lint, type check — use whatever commands the project defines in CLAUDE.md). All must pass.

6. **Post-verify.** If verification fails:
   - If test failures: analyze the failure, fix, re-run. Max 3 fix attempts.
   - If still failing after 3 attempts: spawn `staff-engineer` with test output + plan, re-enter plan mode.
   - If lint/type errors only: fix them directly (these are usually mechanical).

7. **Save memory.** Update MEMORY.md with what was built and any decisions made.

8. **Done.** Summarize what was implemented and any deviations from the plan.
