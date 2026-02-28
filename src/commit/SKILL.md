---
name: commit
description: Create git commits that strictly follow Conventional Commits and Release Please conventions. Validates commit type, scope, description format, breaking change notation, and shows the resulting version bump. Use when committing changes.
argument-hint: "[type] [description]"
---

# Release Please Commit

Create a git commit that strictly follows [Conventional Commits v1.0.0](https://www.conventionalcommits.org/en/v1.0.0/) for use with [Release Please](https://github.com/googleapis/release-please).

## Input

- **Commit type**: `$0` (optional — one of the valid types listed below)
- **Description**: `$1` (optional — brief summary of the change)

If both arguments are provided, use them directly (after validation). If either is missing, infer them from the staged changes and confirm with the user.

## Instructions

1. **Check for staged changes.** Run `git diff --cached --stat` and `git status`. If nothing is staged, show the user what is unstaged and ask what to stage before proceeding. Never run `git add -A` or `git add .` without explicit user approval.

2. **Analyze the changes.** Read the diffs (`git diff --cached`) and recent commit history (`git log --oneline -10`) to understand:
   - What changed and why
   - The commit message style used in this repository

3. **Determine the commit type.** If `$0` was provided and is valid, use it. Otherwise, select the most accurate type from the table in the template and confirm with the user.

4. **Determine the scope (optional).** If the changes are clearly localized to a single module, component, or area of the codebase, suggest a short scope. If the changes span multiple areas, omit the scope. Scopes must be lowercase, short (1-2 words), and describe the code area — never use ticket IDs, issue numbers, or file names as scopes.

5. **Compose the description.** If `$1` was provided, use it (after applying the formatting rules below). Otherwise, write a description that:
   - Uses imperative, present tense ("add" not "added" or "adds")
   - Does not capitalize the first letter
   - Does not end with a period
   - Is concise (under 72 characters for the full header line)
   - Focuses on *what* changed, not *how*

6. **Check for breaking changes.** If the diff introduces any of the following, flag it as a breaking change:
   - Removed or renamed public API, function, class, or endpoint
   - Changed function signatures (required parameters added/removed/reordered)
   - Changed return types or response formats
   - Removed or changed configuration keys or environment variables
   - Dropped support for a runtime, platform, or dependency version

   If a breaking change is detected, ask the user to confirm and provide a `BREAKING CHANGE` description. Apply both the exclamation mark suffix on the type and the `BREAKING CHANGE:` footer.

7. **Compose the body (optional).** Add a body only when the description alone does not fully explain the *why* behind the change. Separate the body from the description with a blank line.

8. **Compose footers (if applicable).** Add footers separated from the body by a blank line:
   - `BREAKING CHANGE: <explanation>` — required when step 6 identified a breaking change. Must be uppercase.
   - `Co-Authored-By:` — include when pair programming or when the user requests it.
   - Other trailers as needed (`Refs:`, `Fixes:`, `Reviewed-by:`).

9. **Validate the full message** against every rule in the Validation Checklist below. If any rule fails, fix the message before proceeding.

10. **Show the user a preview** of the final commit message with:
    - The formatted message in a code block
    - The Release Please version bump this commit would trigger (major, minor, patch, or none)
    - Ask for confirmation before committing

11. **Commit.** Use a heredoc to pass the message to `git commit` to preserve formatting:
    ```
    git commit -m "$(cat <<'EOF'
    <full commit message here>
    EOF
    )"
    ```

12. **Verify.** Run `git log -1 --format="%B"` to confirm the commit was created with the correct message. Run `git status` to confirm a clean working state for staged files.

## Validation Checklist

Before presenting the commit message to the user, verify every rule:

- [ ] **Type is valid**: must be one of the allowed types from the template
- [ ] **Colon-space separator**: a colon and single space follow the type (or scope), e.g., `feat: ` not `feat :` or `feat:`
- [ ] **Scope format** (if present): lowercase, no spaces, wrapped in parentheses, immediately after type — e.g., `feat(auth):`
- [ ] **Description starts lowercase**: first character after `type: ` is not uppercase
- [ ] **Description has no trailing period**: the first line does not end with `.`
- [ ] **Imperative tense**: description uses "add", "fix", "change" — not "added", "fixes", "changed", "adding"
- [ ] **Header length**: the full first line (`type(scope): description`) is at most 72 characters
- [ ] **Blank line after description**: if a body or footer is present, a blank line separates it from the header
- [ ] **BREAKING CHANGE format**: if present, the footer token is exactly `BREAKING CHANGE` (uppercase, space not hyphen) followed by `: `
- [ ] **Breaking mark and footer agree**: if an exclamation mark is used after the type/scope, a `BREAKING CHANGE:` footer is also present
- [ ] **No ticket IDs in scope**: scope does not contain patterns like `JIRA-123`, `#456`, or `GH-789`

## Version Bump Rules

Show the user which Release Please version bump this commit would trigger:

- **Major** (x.0.0) — `BREAKING CHANGE` footer or exclamation mark after type — e.g. `feat!: remove v1 API`
- **Minor** (0.x.0) — `feat` type — e.g. `feat: add search endpoint`
- **Patch** (0.0.x) — `fix` type — e.g. `fix: handle null pointer`
- **Patch** (0.0.x) — `deps` type — e.g. `deps: update axios to 1.6`
- **No release** — any other type (`chore`, `docs`, `ci`, etc.) — e.g. `chore: update gitignore`

Clearly communicate the bump to the user so they can make an informed decision about whether the type accurately reflects their change.

## Template

See [TEMPLATE.md](TEMPLATE.md) for the commit message format and valid types.
