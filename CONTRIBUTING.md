# Contributing

## Branching and PR flow

1. Create feature branches from `develop`.
2. Open pull requests into `develop` for regular work.
3. Run CI and resolve all checks before merge.
4. When ready to release, open a pull request from `develop` to `main`.
5. After merge to `main`, Release Please handles release PR/versioning and `sync-develop` keeps `develop` aligned.

## Commit messages

Use Conventional Commits (for example: `feat: ...`, `fix: ...`, `docs: ...`).

## Skills changes

- Keep canonical skill definitions in `src/<skill>/`.
- Validate `agent-skills list` locally before opening a PR.
