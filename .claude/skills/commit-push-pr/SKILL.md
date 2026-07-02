---
name: commit-push-pr
description: Stage changes, commit with a conventional message, push to remote, and open a pull request. Trigger with /commit-push-pr or a description of the change.
disable-model-invocation: true
---

Arguments: $ARGUMENTS (optional description or commit message hint)

1. Run `git status` to see what's changed and `git diff` to review the changes
2. Stage the relevant files — prefer specific file names over `git add .`
3. Draft a commit message in conventional commit format:
   - Prefix: `feat`, `fix`, `refactor`, `docs`, or `chore`
   - Imperative mood, max 72 chars (e.g., `feat: add price change filter to home page`)
4. Show the staged files and proposed commit message, then ask the user to confirm before committing
5. Run `git commit -m "<message>"` after confirmation
6. Push with `git push -u origin <branch>`
7. If on a non-main branch, run `gh pr create` with a clear title and a brief body describing what changed and why
8. Report the commit hash and PR URL (if created)
