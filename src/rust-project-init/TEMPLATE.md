# Rust Project Init Template

Use the templates below when generating project files. Adjust values based on user input and project kind.

---

## Cargo.toml — Binary

```toml
[package]
name = "project-name"
version = "0.1.0"
edition = "2021"
rust-version = "1.82"
description = "A brief description of the project"
license = "MIT OR Apache-2.0"

[dependencies]
anyhow = "1"

[dev-dependencies]

[lints.rust]
unsafe_code = "forbid"

[lints.clippy]
all = { level = "deny", priority = -1 }
pedantic = { level = "warn", priority = -1 }
nursery = { level = "warn", priority = -1 }
cast_possible_truncation = "allow"
cast_precision_loss = "allow"
cast_sign_loss = "allow"
missing_errors_doc = "allow"
module_name_repetitions = "allow"

[profile.release]
lto = true
strip = true
codegen-units = 1
```

## Cargo.toml — Library

```toml
[package]
name = "project-name"
version = "0.1.0"
edition = "2021"
rust-version = "1.82"
description = "A brief description of the library"
license = "MIT OR Apache-2.0"
readme = "README.md"
repository = ""
keywords = []
categories = []

[dependencies]

[dev-dependencies]

[lints.rust]
unsafe_code = "forbid"

[lints.clippy]
all = { level = "deny", priority = -1 }
pedantic = { level = "warn", priority = -1 }
nursery = { level = "warn", priority = -1 }
cast_possible_truncation = "allow"
cast_precision_loss = "allow"
cast_sign_loss = "allow"
missing_errors_doc = "allow"
module_name_repetitions = "allow"
```

## Cargo.toml — Workspace

```toml
[workspace]
resolver = "2"
members = [
    "crates/*",
]

[workspace.package]
edition = "2021"
rust-version = "1.82"
license = "MIT OR Apache-2.0"

[workspace.dependencies]
# Shared dependencies pinned at workspace level

[workspace.lints.rust]
unsafe_code = "forbid"

[workspace.lints.clippy]
all = { level = "deny", priority = -1 }
pedantic = { level = "warn", priority = -1 }
nursery = { level = "warn", priority = -1 }
cast_possible_truncation = "allow"
cast_precision_loss = "allow"
cast_sign_loss = "allow"
missing_errors_doc = "allow"
module_name_repetitions = "allow"
```

### Workspace Member Cargo.toml

```toml
[package]
name = "member-name"
version = "0.1.0"
edition.workspace = true
rust-version.workspace = true
license.workspace = true

[dependencies]

[dev-dependencies]

[lints]
workspace = true
```

---

## rustfmt.toml

```toml
# Only include settings that differ from defaults.
# See https://rust-lang.github.io/rustfmt/ for all options.
max_width = 100
use_field_init_shorthand = true
```

### Guidelines

- Keep this file minimal. Only override defaults when there is a clear project convention.
- Do not include unstable options unless the project uses nightly rustfmt.

---

## .cargo/config.toml

```toml
[build]
# Uncomment to use a faster linker (install separately):
# [target.x86_64-unknown-linux-gnu]
# linker = "clang"
# rustflags = ["-C", "link-arg=-fuse-ld=lld"]

# [target.aarch64-apple-darwin]
# rustflags = ["-C", "link-arg=-fuse-ld=/opt/homebrew/opt/llvm/bin/ld64.lld"]

[alias]
ck = "clippy --all-targets --all-features -- -D warnings"
ta = "test --all-features"
```

### Guidelines

- Include linker configuration as comments — let the user opt in based on their platform.
- Provide useful aliases for common development workflows.

---

## src/main.rs — Binary

```rust
//! Brief description of what this binary does.

use anyhow::Result;

fn main() -> Result<()> {
    println!("Hello from project-name!");
    Ok(())
}
```

## src/lib.rs — Library

```rust
//! Brief description of what this library provides.
//!
//! # Overview
//!
//! Describe the library's purpose and primary types/functions.

// Modules will be declared here as the project grows.
// mod module_name;
```

---

## .gitignore

```
# Rust build artifacts
/target

# Editor and OS files
.DS_Store
*.swp
*.swo
*~
.idea/
.vscode/
*.iml
```

### Guidelines

- For **libraries**: add `Cargo.lock` to `.gitignore` (consumers use their own lockfile).
- For **binaries** and **workspaces with binaries**: commit `Cargo.lock` (reproducible builds).

---

## README.md

````md
# project-name

Brief description of the project.

## Build

```bash
cargo build
```

## Test

```bash
cargo test
```

## Lint

```bash
cargo clippy --all-targets --all-features -- -D warnings
```

## Format

```bash
cargo fmt
```
````

---

## Verification Checklist

After generating all files, verify:

- [ ] `cargo check` compiles without errors
- [ ] `cargo clippy -- -D warnings` produces no warnings
- [ ] `cargo fmt --check` produces no diff
- [ ] `cargo test` passes (at least the default test)
- [ ] `.gitignore` includes `/target`
- [ ] `Cargo.lock` handling matches project kind (committed for binaries, ignored for libraries)
- [ ] `rust-version` in `Cargo.toml` matches the user's requested or inferred MSRV
