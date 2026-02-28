---
name: pr-description
description: Draft high-quality pull request titles and bodies from the branch diff, commit history, and test results. Use when opening or updating a PR.
argument-hint: "[base-branch]"
---

# Pull Request Description Generator

Generate a concise, reviewer-friendly PR title and body that explain *why* the change exists, what changed, risks, and validation.

## Input

- **Base branch**: `$0` (optional; default to `main`)

If omitted, detect the repository default branch from git metadata. Fall back to `main`.

## Instructions

1. Inspect current branch state using:
   - `git status`
   - `git log --oneline <base>..HEAD`
   - `git diff --stat <base>...HEAD`
   - `git diff <base>...HEAD`
2. Identify the primary intent of the PR (feature, fix, refactor, docs, test, chore).
3. Create a PR title in imperative style, <= 72 chars when possible.
4. Generate body sections using the template:
   - Summary (1-3 bullets)
   - Why this change
   - Testing
   - Risks / rollout notes (only if applicable)
   - Checklist
5. If tests were not run, explicitly state that and suggest the exact command(s).
6. If there are breaking changes, add a clearly labeled section with migration guidance.
7. Keep wording factual and specific; avoid generic filler.

## Output rules

- Prefer concrete file- or behavior-level outcomes.
- Mention notable deleted/renamed paths when relevant.
- Keep body skimmable; short bullets over long paragraphs.

## Template

See [TEMPLATE.md](TEMPLATE.md) for the output format.
