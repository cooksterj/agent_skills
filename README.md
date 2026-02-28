# Agent Skills

A collection of reusable agent skills that work across:

- Claude (`.claude/skills`)
- OpenAI Codex (`.agents/skills`)
- OpenCode (`.opencode/skills`)

Skills are authored once under `src/` and installed into target repositories using the `agent-skills` CLI.

## Setup

Add this repo to your shell `PATH` (one-time). Add this to `~/.zshrc` or `~/.bashrc`:

```bash
export PATH="$PATH:/path/to/agent_skills"
```

## Usage

Run from the root of any target repository:

```bash
# Show available skills and install status across all targets
agent-skills list

# Show status for a specific target
agent-skills list --target codex

# Install all skills to all targets
agent-skills install

# Install specific skills to all targets
agent-skills install commit pr-review

# Install specific skills only for Codex
agent-skills install --target codex commit pr-description

# Update installed skills (all targets by default)
agent-skills update

# Update only OpenCode target
agent-skills update --target opencode

# Remove a skill from a target
agent-skills uninstall --target claude commit
```

## Targets

- `claude` -> `.claude/skills/`
- `codex` -> `.agents/skills/`
- `opencode` -> `.opencode/skills/`
- `all` -> all of the above (default)

## Skills

### Existing

- `commit`: Conventional Commits + Release Please commit workflow
- `jira-ticket`: standardized Jira ticket generation (epic/story/task/sub-task)
- `python-scaffold`: Python scaffolding with docstring/test conventions

### Added

- `pr-description`: structured PR title/body generation from branch changes
- `pr-review`: rubric-based PR review with severity and actionable fixes
- `test-plan`: risk-based test matrix with edge/failure scenarios
- `release-notes`: audience-friendly release notes aligned with Release Please

## Release Please interaction

Release Please remains the source of truth for:

- version bumping
- release PR creation
- canonical `CHANGELOG.md` updates

The `release-notes` and `commit` skills improve input quality and narrative clarity, but do not replace Release Please automation.

## PR flow

- Create feature branches from `develop`
- Open PRs into `develop` for day-to-day work
- Promote `develop` to `main` with a release PR
- `sync-develop` keeps `develop` aligned after `main` merges
