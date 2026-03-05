---
name: rust-error-type
description: Scaffold idiomatic Rust error types using thiserror (libraries) or anyhow (binaries) with proper Display, From impls, and conversion logic. Use when adding error handling to Rust code.
argument-hint: "[error-name] [brief description]"
---

# Rust Error Type Generator

Generate idiomatic Rust error types with proper trait implementations, conversions, and test coverage.

## Input

- **Error name**: `$0` (optional — the name for the error type, e.g., `ParseError`, `ConfigError`)
- **Description**: `$1` (optional — brief description of what operations can produce this error)

If both are missing, analyze the current code context to infer what error type is needed. If the context is insufficient, ask the user.

## Instructions

1. **Determine the error context.** Scan the project to understand:
   - Is this a library or a binary? Check `Cargo.toml` for `[[bin]]` vs `[lib]` sections.
   - Does the project already use `thiserror`, `anyhow`, or manual `impl Error`?
   - Are there existing error types? If so, match their style and consider whether the new error should be a variant of an existing type or a new top-level type.
   - What error types from dependencies are commonly encountered? (e.g., `std::io::Error`, `serde_json::Error`, `reqwest::Error`)

2. **Choose the approach.**
   - **Library code**: use `thiserror` for custom error enums. Libraries must expose typed errors so consumers can match on variants.
   - **Binary/application code**: use `anyhow` for error propagation. Binaries typically do not need typed errors at the top level — they report errors to the user.
   - **Mixed (library with a binary frontend)**: use `thiserror` in the library crate, `anyhow` in the binary crate, with `From` conversions bridging them.
   - **No external dependencies requested**: implement `std::fmt::Display` and `std::error::Error` manually.

3. **Design the error enum.** For `thiserror`-based errors:
   - Create one variant per distinct failure mode the caller might want to handle differently.
   - Group related errors under a single variant when the caller does not need to distinguish between them.
   - Use `#[from]` for direct 1:1 conversions from source errors.
   - Use `#[source]` when wrapping an error with additional context.
   - Add context fields (e.g., file path, key name) so error messages are actionable.

4. **Generate the error type** following the template structure.

5. **Generate conversions.** Add `From` implementations for source errors that do not map 1:1 (where `#[from]` is insufficient because context fields need to be populated).

6. **Generate tests** covering:
   - Every variant can be constructed
   - `Display` output matches expected format
   - `source()` returns the correct inner error
   - `From` conversions work for all source types
   - Error type is `Send + Sync + 'static` (required for use with `anyhow` and across threads)

7. **Integrate with existing code.** If the user has existing functions that use `unwrap()`, `expect()`, `panic!`, or return `String` errors, suggest refactoring them to use the new error type. Present the refactoring as a recommendation, not an automatic change.

8. **Present the generated code** to the user for review.

## Error Design Principles

- **One error enum per module or logical boundary.** Avoid a single giant error enum for the entire crate.
- **Variants describe what failed, not how it failed.** Use `ConfigNotFound` not `IoError`; the `#[source]` chain provides the "how."
- **Include context in error messages.** `"config file not found: {path}"` is more useful than `"file not found"`.
- **Keep error types `Send + Sync + 'static`.** This is required for multithreaded use and compatibility with `anyhow::Error`.
- **Avoid stringly-typed errors.** Do not use `String` as an error type. Use a proper enum variant even for "other" cases.
- **Use `#[non_exhaustive]`** on public error enums so new variants can be added without a semver-breaking change.

## When to Use Each Pattern

| Scenario | Pattern | Crate |
|----------|---------|-------|
| Library with distinct failure modes callers handle | `thiserror` enum | `thiserror` |
| Binary that propagates errors to the user | `anyhow::Result` | `anyhow` |
| Library that wraps a single source error | newtype with `thiserror` | `thiserror` |
| Quick prototyping / scripts | `anyhow::Result` | `anyhow` |
| No external dependency allowed | manual `Display` + `Error` impl | (none) |
| Error with complex recovery logic | `thiserror` enum + methods on the error type | `thiserror` |

## Template

See [TEMPLATE.md](TEMPLATE.md) for error type formats.
