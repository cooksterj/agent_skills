---
name: rust-scaffold
description: Generate idiomatic Rust structs, enums, traits, functions, or modules with rustdoc comments, derive macros, and complete test coverage that match the project's existing conventions. Use when scaffolding new Rust code.
argument-hint: "[type] [brief description]"
---

# Rust Scaffold Generator

Generate a minimal, idiomatic Rust scaffold and its corresponding tests using the template below. Every scaffold must include rustdoc comments, appropriate `#[derive]` attributes, and complete test coverage.

## Input

- **Scaffold type**: `$0` (must be one of: `struct`, `enum`, `trait`, `function`, `module`)
- **Description**: `$1` (brief description of what the code should do)

If the scaffold type or description is missing, ask the user for the missing information before generating.

## Instructions

1. Determine the scaffold type from the arguments. If not provided or not one of `struct`, `enum`, `trait`, `function`, `module`, ask the user.
2. Before generating, scan the target project for existing conventions:
   - Module structure and `mod`/`use` organization
   - Naming conventions (`snake_case` for functions/modules, `PascalCase` for types/traits, `SCREAMING_SNAKE_CASE` for constants)
   - Error handling patterns (`Result`, `Option`, custom error types, `thiserror`, `anyhow`)
   - Common derive macros used in the project (`Debug`, `Clone`, `Serialize`, etc.)
   - Visibility patterns (`pub`, `pub(crate)`, `pub(super)`)
   - `Cargo.toml` for edition, dependencies, and feature flags
   - Test patterns (inline `#[cfg(test)]` modules, `tests/` directory, assertion style)
   - Whether the project uses `clippy` lints or `rustfmt` configuration
3. Generate the scaffold following the template structure, tailored to the scaffold type.
4. Generate complete tests covering every public method, function, variant, and branch.
5. Place the code where it logically fits within the existing project structure. If the location is ambiguous, ask the user.
6. Present the generated code and tests to the user for review.

## Rustdoc Rules

- Use `///` for all public items (structs, enums, traits, functions, methods, fields where clarification is needed).
- Use `//!` for module-level documentation at the top of the file.
- Start with a single-line summary sentence.
- Add a blank `///` line, then extended description only when the summary is insufficient.
- Include `# Examples` sections only when the user explicitly requests examples.
- Include `# Errors`, `# Panics`, and `# Safety` sections only when applicable.
- Use `# Arguments` sparingly — prefer self-documenting parameter names and types.

## Ownership and Borrowing Rules

- Prefer borrowing (`&self`, `&T`) over ownership when the function does not need to consume the value.
- Use `&str` for string parameters, `String` for owned fields.
- Use `impl Into<String>` or `impl AsRef<str>` for constructors that store owned strings, so callers can pass either `&str` or `String`.
- Prefer returning owned types from constructors and `&T` from accessor methods.
- Use `Cow<'_, str>` only when the function genuinely needs to sometimes borrow and sometimes own.
- Never clone to satisfy the borrow checker without a comment explaining why.

## Derive and Attribute Rules

- Always derive `Debug` on public types.
- Derive `Clone`, `PartialEq`, `Eq`, `Hash` when the type semantically supports equality and hashing.
- For data types: prefer `#[derive(Debug, Clone, PartialEq, Eq)]` as a baseline.
- For enums with variants that carry data: derive only what all variants can support.
- Add `#[non_exhaustive]` on public enums and structs exposed as library API to preserve semver flexibility.
- Use `#[must_use]` on functions that return values callers should not silently discard.
- Match the project's existing `serde` usage — only add `Serialize`/`Deserialize` when the project already depends on serde or the user requests it.

## Visibility Rules

- Default to the minimum necessary visibility.
- Use `pub(crate)` for items shared within the crate but not exposed externally.
- Use `pub` only for items intended as public API.
- Keep helper functions private (`fn`, not `pub fn`) unless there is a reason to expose them.

## Structure Rules

- Keep each scaffold in its own file (or module directory for `module` type).
- Re-export public types from the parent module's `mod.rs` or `lib.rs` as appropriate.
- Group `impl` blocks logically: inherent methods first, then trait implementations.
- Place `#[cfg(test)]` module at the bottom of the file.

## Test Rules

- Generate a `#[cfg(test)]` module in the same file for unit tests.
- Provide complete test coverage: every public method, function, enum variant, and code branch must have at least one test.
- Include tests for error conditions and `Result::Err` paths.
- For traits, create a minimal mock/stub implementation in the test module to verify the trait contract.
- Use `#[test]` functions with descriptive names: `test_<function>_<scenario>` (e.g., `test_new_with_empty_name_returns_error`).
- Use `assert_eq!`, `assert_ne!`, `assert!(matches!(...))`, and `#[should_panic]` as appropriate.
- Use `-> Result<(), Box<dyn std::error::Error>>` for tests that need the `?` operator.
- If the project uses `proptest` or `quickcheck`, include property-based tests where meaningful.
- Match the project's existing test patterns. Default to inline `#[cfg(test)] mod tests` when no convention exists.

## Generation Rules

- Produce a minimal skeleton the developer fills in. Do not generate speculative business logic.
- Only include `use` statements that are actually used by the scaffold.
- Match the project's existing style. When no project context exists, default to Rust 2021 edition idioms.
- Prefer `todo!()` over `unimplemented!()` for stub bodies, as it conveys intent to fill in later.
- Use `Self` instead of repeating the type name inside `impl` blocks.

## Template

See [TEMPLATE.md](TEMPLATE.md) for the scaffold format by type.
