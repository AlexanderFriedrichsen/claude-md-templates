description: Write tests first from a spec, then implement until they pass (TDD workflow).
argument-hint: "<feature description or spec>"

1. **Understand.** Read relevant existing code to understand patterns, types, and conventions.
2. **Write tests.** Based on the feature description, write failing tests in the appropriate test file. Tests should cover:
   - Happy path
   - Edge cases (empty input, undefined, boundary values)
   - Error conditions
3. **Confirm.** Run the test file to verify all new tests FAIL (not error — fail).
4. **Implement.** Write the minimum code to make tests pass. Do not over-engineer.
5. **Verify.** Run full test suite + linter.
6. **Refactor.** If the implementation is messy, clean it up. Re-run verification.
7. **Report.** List tests written, what they cover, and final pass/fail status.
