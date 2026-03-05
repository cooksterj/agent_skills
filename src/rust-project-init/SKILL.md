---
name: rust-project-init
description: Scaffold a new Rust project or Cargo workspace with best-practice configuration including clippy lints, rustfmt settings, CI-ready Cargo.toml, and idiomatic project layout. Use when starting a new Rust project.
argument-hint: "[project-name] [binary|library|workspace]"
---

# Rust Project Initializer

Scaffold a new Rust project with production-ready configuration and idiomatic project structure.

## Input

- **Project name**: `$0` (required — name of the project, must be a valid Rust crate name)
- **Project kind**: `$1` (optional — one of: `binary`, `library`, `workspace`; default `binary`)

If the project name is missing, ask the user. If the kind is missing, default to `binary`.

## Instructions

1. **Validate the project name.** Must follow Rust crate naming conventions:
   - Lowercase alphanumeric and hyphens only
   - Must start with a letter
   - No consecutive hyphens, no trailing hyphens
   - If the user provides a name with underscores, suggest the hyphenated equivalent (Cargo convention) and confirm.

2. **Determine the project kind.** If `$1` was provided and is valid, use it. Otherwise default to `binary`. If the user's description implies both a library and a binary (e.g., "CLI tool with a reusable core"), suggest `workspace` or a binary with `lib.rs` and confirm.

3. **Run `cargo init` or `cargo new`.** Use the appropriate command:
   - `binary`: `cargo new <name>` (default template)
   - `library`: `cargo new --lib <name>`
   - `workspace`: create root `Cargo.toml` with `[workspace]`, then `cargo new` for each member crate

4. **Configure `Cargo.toml`.** Enhance the generated `Cargo.toml` with:
   - `rust-version` set to the current MSRV (minimum supported Rust version) — default to latest stable minus one (e.g., `"1.82"` if stable is 1.83). Ask the user if they have a specific MSRV requirement.
   - `[lints.rust]` and `[lints.clippy]` sections with recommended lints (see template).
   - `[profile.release]` with `lto = true` and `strip = true` for binaries.
   - `edition = "2021"` (or `"2024"` if the user requests it and their toolchain supports it).

5. **Create `rustfmt.toml`.** Generate a minimal rustfmt configuration (see template). Only include settings that differ from defaults.

6. **Create `clippy.toml`.** Generate a clippy configuration if the project needs non-default settings (e.g., `cognitive-complexity-threshold`). Skip if defaults are sufficient.

7. **Create `.cargo/config.toml`.** Add recommended Cargo configuration (see template).

8. **Scaffold the source layout.** Based on project kind:
   - `binary`: `src/main.rs` with a minimal main function and module structure comment
   - `library`: `src/lib.rs` with module-level documentation and a placeholder public type
   - `workspace`: root workspace manifest plus member crates with appropriate structure

9. **Set up error handling.** For all project kinds:
   - For binaries: add `anyhow` to `[dependencies]` and set up `fn main() -> anyhow::Result<()>`
   - For libraries: mention `thiserror` as the recommended approach for custom error types, but do not add it unless the user requests it

10. **Create a `.gitignore`.** Add a Rust-appropriate `.gitignore` if one does not already exist. Include `/target`, `Cargo.lock` (for libraries only — binaries should commit it), and common editor/OS patterns.

11. **Create a basic README.** Generate a minimal `README.md` with the project name, a one-line description placeholder, and build/test commands.

12. **Verify the setup.** Run:
    - `cargo check` — confirm the project compiles
    - `cargo clippy -- -D warnings` — confirm no clippy warnings
    - `cargo fmt --check` — confirm formatting is clean
    - `cargo test` — confirm tests pass (even if just the default placeholder test)

13. **Present a summary** to the user listing:
    - Files created and their purpose
    - Key configuration choices made
    - Next steps (add dependencies, create modules, etc.)

## Naming Conventions

Communicate these Rust naming conventions to the user when relevant:

| Item | Convention | Example |
|------|-----------|---------|
| Crate name | `kebab-case` | `my-cool-crate` |
| Module name | `snake_case` | `my_module` |
| Type/Trait | `PascalCase` | `MyStruct`, `MyTrait` |
| Function/Method | `snake_case` | `do_something` |
| Constant | `SCREAMING_SNAKE_CASE` | `MAX_RETRIES` |
| Static | `SCREAMING_SNAKE_CASE` | `DEFAULT_CONFIG` |
| Type parameter | Short `PascalCase` | `T`, `K`, `V`, `Item` |
| Lifetime | Short `lowercase` | `'a`, `'de`, `'src` |
| Feature flag | `kebab-case` | `serde-support` |

## Template

See [TEMPLATE.md](TEMPLATE.md) for configuration file formats.
