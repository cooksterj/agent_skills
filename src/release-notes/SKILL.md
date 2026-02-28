---
name: release-notes
description: Draft human-readable release notes from conventional commits and merged pull requests while staying aligned with Release Please generated changelogs.
argument-hint: "[from-ref] [to-ref]"
---

# Release Notes Drafting Skill

Produce clear release notes intended for humans (customers, developers, or internal teams) while preserving Release Please as the source of truth for versioning and changelog automation.

## Input

- **From ref**: `$0` (optional; previous tag or commit)
- **To ref**: `$1` (optional; default `HEAD`)

If refs are missing, infer from latest tag to `HEAD`.

## Instructions

1. Collect release context with:
   - `git tag --sort=-creatordate`
   - `git log --oneline <from>..<to>`
   - `git log --merges --oneline <from>..<to>` (if PR merge flow exists)
2. Group changes into audience-friendly categories:
   - Features
   - Fixes
   - Performance
   - Developer Experience / Tooling
   - Breaking Changes (if any)
3. Rewrite terse commit subjects into meaningful outcome-focused bullets.
4. Call out migration actions for any breaking changes.
5. Add a "Known issues" section only when evidence exists.
6. Keep notes consistent with (not contradictory to) Release Please changelog data.

## Interaction with Release Please

- Release Please controls version bumps and canonical `CHANGELOG.md` updates.
- This skill provides narrative summaries for release announcements, PR descriptions, and stakeholder communication.
- Do not invent changes absent from commit/PR history.

## Template

See [TEMPLATE.md](TEMPLATE.md).
