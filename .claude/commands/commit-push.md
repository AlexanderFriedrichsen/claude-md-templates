description: Stage changes, write a conventional commit message, and push.
argument-hint: "<optional: override commit message>"

1. Run `git diff --stat` to see what changed.
2. Run `git diff` to understand the content of changes.
3. Write a conventional commit message based on the actual changes. Format: `type: concise description` (50 char max subject line).
   - If the human provided a message override, use that instead.
4. Run `git add -A`.
5. Run `git commit -m "<message>"`.
6. Run `git push`.
7. Report: what was committed, to which branch, and the commit hash.
