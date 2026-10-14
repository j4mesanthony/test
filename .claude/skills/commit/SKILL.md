---
name: commit
description: Commit changes using short Conventional Commits messages (type prefix, title, bullet summary). Use when the user asks to commit, save changes to git, or invokes /commit.
allowed-tools: Bash(git status:*), Bash(git diff:*), Bash(git log:*), Bash(git add:*), Bash(git commit:*)
---

# Commit

Create git commits whose messages follow the [Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/#summary) specification.

## Message format

```
<type>[optional scope][!]: <Short Title>
- <Short bullet of a change>
- <Short bullet of a change>
```

Example:

```
feat: Initialise Project
- Added package.json
- Installed TypeScript
```

## Rules

- **Type prefix is required**, lowercase, one of:
  - `feat` – new feature (SemVer MINOR)
  - `fix` – bug fix (SemVer PATCH)
  - `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`
- Optional scope in parentheses after the type, e.g. `feat(auth): Add Login`.
- Colon and a single space follow the type/scope, then the title.
- Title is short (aim for ≤ 50 characters), imperative, Title Case, no trailing period.
- A blank line is not required between title and bullets in this project; keep the bullets directly under the title as in the example.
- Bullets: 2–5 short lines starting with `- ` and a past-tense verb (Added, Installed, Fixed, Removed...). Describe what changed, not why at length. Skip bullets if the title says it all.
- **Breaking changes**: add `!` before the colon (`feat!: Drop Node 16`) and/or a `BREAKING CHANGE: <description>` footer.
- Never mix unrelated changes in one commit. Split into several commits, each with its own type.
- Keep it concise: no essays, no file-by-file dumps.

## Workflow

1. Run `git status` and `git diff` (staged and unstaged) to see what changed. Run `git log --oneline -5` to match recent style.
2. If the changes are unrelated, group them into separate logical commits.
3. Stage only the relevant files by name (`git add <files>`). Do not use `git add -A` or `git add .`, and never stage secrets (`.env`, credentials) or build output (`node_modules`, `dist`).
4. Choose the correct type from the actual change, then write the message in the format above.
5. Commit with a heredoc so newlines are preserved:
   ```
   git commit -m "$(cat <<'EOF'
   feat: Add Thing
   - Added thing
   EOF
   )"
   ```
   If the environment/system instructions require an attribution trailer (e.g. `Co-Authored-By`), append it after a blank line.
6. Run `git status` to confirm, and report the commit hash and message.

## Guardrails

- Do not push, amend, rebase, or force anything unless the user asks.
- Never skip hooks (`--no-verify`). If a hook fails, fix the issue and create a new commit.
- If the type, grouping, or scope is unclear, ask the user before committing.
- If there is nothing to commit, say so.
