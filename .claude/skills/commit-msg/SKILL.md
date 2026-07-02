---
name: commit-msg
description: Generate a conventional commit message from staged changes and commit. Trigger with /commit-msg, or when the user says "write a commit message", "generate a commit", or "commit my changes".
---

Arguments: $ARGUMENTS (optional hint about the change)

1. Run `git diff --staged --stat` to check whether anything is staged. If the output is empty, stop immediately and tell the user to stage their changes first (`git add <files>`). Do not proceed.

2. Run `git diff --staged` to read the full staged diff.

3. Derive a commit message from the diff:

   **Format:**
   ```
   type(scope): short subject

   - bullet describing what changed
   - bullet describing why (if non-obvious)
   ```

   **Rules:**
   - `type` must be one of: `feat`, `fix`, `refactor`, `chore`, `docs`, `style`, `test`
   - `scope` is the area of the codebase affected (e.g. `home`, `CoinCard`, `css`, `auth`) — omit parentheses if scope is unclear
   - Subject line must be under 60 characters, imperative mood, no trailing period
   - Body bullets are optional but encouraged when the why isn't obvious from the subject
   - Never add a `Co-Authored-By` trailer or any other trailers

4. Run `git commit` with the generated message passed via heredoc to preserve formatting:
   ```
   git commit -m "$(cat <<'EOF'
   type(scope): short subject

   - bullet one
   - bullet two
   EOF
   )"
   ```

5. Report the commit hash and the full message that was used.
