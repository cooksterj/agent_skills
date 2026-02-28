---
name: pr-review
description: Perform structured pull request reviews with clear findings, severity levels, and actionable fixes covering correctness, tests, security, and maintainability.
argument-hint: "[base-branch]"
---

# Pull Request Review Skill

Review a branch or working tree with a consistent rubric and produce actionable feedback.

## Input

- **Base branch**: `$0` (optional; default `main`)

## Instructions

1. Gather context from:
   - `git status`
   - `git log --oneline <base>..HEAD`
   - `git diff <base>...HEAD`
2. Evaluate changes across these categories:
   - Correctness and logic bugs
   - Error handling and edge cases
   - Test coverage gaps
   - Security/privacy concerns
   - Performance regressions
   - Maintainability/readability
3. Report findings with severity labels:
   - `high`: likely defect, data/security risk, or breaking behavior
   - `medium`: meaningful quality or reliability concern
   - `low`: improvement suggestion
4. For each finding, include:
   - What is wrong
   - Why it matters
   - Specific recommendation
5. If no issues are found, state "no major issues found" and still mention residual risks or test follow-ups.

## Output rules

- Be specific and evidence-based.
- Avoid vague commentary.
- Prefer concise bullets grouped by severity.

## Template

See [TEMPLATE.md](TEMPLATE.md).
