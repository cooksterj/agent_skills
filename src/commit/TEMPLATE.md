# Commit Message Template

Use this format for every commit message. Replace placeholder text with content derived from the staged changes.

---

## Message Format

```
<type>[(scope)][!]: <description>

[body]

[footer(s)]
```

### Header (first line)

```
type(scope): description
```

- **type**: required — one of the valid types below
- **scope**: optional — lowercase identifier for the code area affected (e.g., `auth`, `parser`, `api`, `ui`)
- **!**: optional — append immediately after type or scope to signal a breaking change
- **description**: required — imperative, present tense, lowercase start, no trailing period

The entire header line must be **72 characters or fewer**.

### Body (optional)

Separated from the header by a **blank line**. Provides additional context about *why* the change was made. Wrap lines at 100 characters.

### Footer(s) (optional)

Separated from the body (or header if no body) by a **blank line**. Each footer follows `token: value` or `token #value` format. Footer tokens use `-` in place of spaces, with one exception: `BREAKING CHANGE` must use a space and be uppercase.

---

## Valid Commit Types

| Type | Purpose | Release Please Bump |
|------|---------|---------------------|
| `feat` | A new feature or capability | **Minor** |
| `fix` | A bug fix | **Patch** |
| `docs` | Documentation-only changes | No release |
| `style` | Code style (formatting, whitespace, semicolons) — no logic change | No release |
| `refactor` | Code restructuring without behavior change | No release |
| `perf` | Performance improvement | No release |
| `test` | Adding or correcting tests | No release |
| `build` | Build system or external dependency changes | No release |
| `ci` | CI/CD configuration changes | No release |
| `chore` | Maintenance, tooling, or other non-production changes | No release |
| `revert` | Revert a previous commit | Varies |
| `deps` | Dependency updates | **Patch** |

---

## Breaking Change Notation

Use **both** methods together for maximum clarity:

1. Append an exclamation mark after the type (or scope):
   ```
   feat(api)!: require OAuth2 tokens for all endpoints
   ```

2. Add a `BREAKING CHANGE:` footer:
   ```
   feat(api)!: require OAuth2 tokens for all endpoints

   Migrate all clients from API key authentication to OAuth2.

   BREAKING CHANGE: API key authentication has been removed. All clients must use OAuth2 bearer tokens.
   ```

---

## Examples

### Simple feature (minor bump)

```
feat: add full-text search to project listing
```

### Bug fix with scope (patch bump)

```
fix(parser): handle null input without raising TypeError
```

### Chore with no release

```
chore: update pre-commit hooks to latest versions
```

### Breaking change (major bump)

```
feat(auth)!: replace session-based auth with JWT tokens

Session cookies are no longer issued. All clients must include
a valid JWT in the Authorization header.

BREAKING CHANGE: session-based authentication has been removed. Clients must migrate to JWT bearer tokens before upgrading.
```

### Commit with body and co-author

```
fix(api): return 404 instead of 500 for missing resources

The API was raising an unhandled KeyError when a resource ID did not
exist, resulting in a 500 response. This catches the KeyError and
returns a proper 404 with an error message.

Co-Authored-By: Jane Doe <jane@example.com>
```

### Dependency update (patch bump)

```
deps: update lodash from 4.17.20 to 4.17.21
```

### Revert

```
revert: feat: add full-text search to project listing

This reverts commit abc1234. The search index was causing
out-of-memory errors in production.
```

---

## Common Mistakes to Avoid

| Mistake | Wrong | Correct |
|---------|-------|---------|
| Past tense | `feat: added search` | `feat: add search` |
| Capitalized description | `feat: Add search` | `feat: add search` |
| Trailing period | `fix: handle null.` | `fix: handle null` |
| Missing space after colon | `feat:add search` | `feat: add search` |
| Ticket ID as scope | `feat(JIRA-123): add search` | `feat(search): add search` |
| Lowercase BREAKING CHANGE | `breaking change: removed API` | `BREAKING CHANGE: removed API` |
| No blank line before footer | `feat: change API\nBREAKING CHANGE: x` | `feat: change API\n\nBREAKING CHANGE: x` |
| Non-releasable type for new feature | `chore: add new utility function` | `feat: add new utility function` |
