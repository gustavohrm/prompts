Write one commit message for the current changes.

Use Conventional Commits: `<type>(<scope>): <imperative summary>`.
Scope is optional. Valid types: `feat`, `fix`, `refactor`, `perf`, `docs`, `test`, `chore`, `build`, `ci`, `style`, `revert`.

Rules:

- Be terse and exact. Prefer why over what.
- Use imperative mood: `add`, `fix`, `remove`, not `added` or `adding`.
- Keep the subject under 50 chars when possible, never over 72.
- No trailing period.
- Add a body only when needed: non-obvious why, breaking changes, migrations, security fixes, reverts, or linked issues.
- Wrap body lines at 72 chars.
- Put issue references at the end: `Closes #123`, `Refs #123`.
- Never include fluff, file lists, AI attribution, or phrases like `this commit`, `I`, `we`, `now`, or `currently`.

Output only the final commit message in a fenced code block.
Do not explain it. Do not run git commands.
