# Rust Error Type Template

Use the section below matching the chosen approach. Replace placeholder text with content derived from the user's description and the project's conventions.

---

## thiserror — Enum (primary pattern for libraries)

```rust
use std::path::PathBuf;

use thiserror::Error;

/// Errors that can occur during [operation description].
#[derive(Debug, Error)]
#[non_exhaustive]
pub enum ErrorName {
    /// The configuration file was not found at the expected path.
    #[error("config file not found: {path}")]
    ConfigNotFound {
        /// The path that was searched.
        path: PathBuf,
    },

    /// Failed to parse the configuration file.
    #[error("failed to parse config: {path}")]
    ParseFailed {
        /// The path of the file that failed to parse.
        path: PathBuf,
        /// The underlying parse error.
        #[source]
        source: serde_json::Error,
    },

    /// An I/O error occurred while reading or writing.
    #[error(transparent)]
    Io(#[from] std::io::Error),

    /// A validation rule was violated.
    #[error("{field}: {message}")]
    Validation {
        /// The field that failed validation.
        field: String,
        /// Description of the validation failure.
        message: String,
    },
}
```

### Guidelines

- **`#[error("...")]`**: defines the `Display` impl for each variant. Use format strings with named fields for clarity.
- **`#[from]`**: auto-generates `From<SourceError>` for 1:1 conversions. Use on at most one field per source error type.
- **`#[source]`**: marks a field as the error source for the `Error::source()` chain. Use when the conversion needs additional context fields beyond just the source error.
- **`#[error(transparent)]`**: delegates both `Display` and `source()` to the inner error. Use for simple newtype wrappers.
- **`#[non_exhaustive]`**: always add on public error enums to allow adding variants without a semver break.
- Only include variants for failure modes that actually exist. Do not generate speculative variants.

---

## thiserror — Newtype Wrapper

Use when wrapping a single error source with additional context.

```rust
use thiserror::Error;

/// An error that occurred during [operation description].
#[derive(Debug, Error)]
#[error("failed to {operation}: {source}")]
pub struct ErrorName {
    /// What operation was being attempted.
    pub operation: String,
    /// The underlying error.
    #[source]
    pub source: SourceErrorType,
}

impl ErrorName {
    /// Create a new error for the given operation.
    pub(crate) fn new(operation: impl Into<String>, source: SourceErrorType) -> Self {
        Self {
            operation: operation.into(),
            source,
        }
    }
}
```

### Guidelines

- Use newtype wrappers when there is exactly one failure mode but the caller benefits from additional context.
- Keep the constructor `pub(crate)` unless external consumers need to create instances.

---

## anyhow — Binary Error Handling

For binary crates, use `anyhow::Result` as the return type and `.context()` to add meaning to errors.

```rust
use anyhow::{Context, Result};

fn load_config(path: &std::path::Path) -> Result<Config> {
    let content = std::fs::read_to_string(path)
        .with_context(|| format!("failed to read config file: {}", path.display()))?;

    let config: Config = serde_json::from_str(&content)
        .with_context(|| format!("failed to parse config file: {}", path.display()))?;

    Ok(config)
}

fn main() -> Result<()> {
    let config = load_config("config.json".as_ref())?;
    // ...
    Ok(())
}
```

### Guidelines

- Use `.context("static message")` when the context is a fixed string.
- Use `.with_context(|| format!(...))` when the context includes runtime values (avoids allocation on the happy path).
- Add context at every level where it would help a human understand what went wrong.
- In `main()`, `anyhow` automatically prints the full error chain when returning `Err`.
- For CLI applications using `clap`, integrate `anyhow` in the main function and let clap handle argument errors separately.

---

## Manual Implementation (no external dependencies)

```rust
use std::fmt;

/// Errors that can occur during [operation description].
#[derive(Debug)]
#[non_exhaustive]
pub enum ErrorName {
    /// Description of this failure mode.
    VariantA { field: String },
    /// Description of this failure mode.
    VariantB(std::io::Error),
}

impl fmt::Display for ErrorName {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        match self {
            Self::VariantA { field } => write!(f, "operation failed for: {field}"),
            Self::VariantB(err) => write!(f, "I/O error: {err}"),
        }
    }
}

impl std::error::Error for ErrorName {
    fn source(&self) -> Option<&(dyn std::error::Error + 'static)> {
        match self {
            Self::VariantA { .. } => None,
            Self::VariantB(err) => Some(err),
        }
    }
}

impl From<std::io::Error> for ErrorName {
    fn from(err: std::io::Error) -> Self {
        Self::VariantB(err)
    }
}
```

### Guidelines

- Implement `Display` with human-readable messages.
- Implement `Error::source()` to return the inner error when one exists.
- Implement `From<SourceError>` for each source type to enable the `?` operator.
- This is more verbose than `thiserror` but avoids a proc-macro dependency.

---

## Convenience Type Alias

Define a `Result` type alias local to the module or crate to reduce boilerplate.

```rust
/// A specialized `Result` type for this module.
pub type Result<T> = std::result::Result<T, ErrorName>;
```

### Guidelines

- Place in the module root or `lib.rs` alongside the error type.
- Only create one `Result` alias per error type — do not shadow `std::result::Result` in modules that use multiple error types.
- Name it `Result` (not `MyResult`) so it reads naturally: `fn do_thing() -> Result<Value>`.

---

## Test Module

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_display_config_not_found() {
        let err = ErrorName::ConfigNotFound {
            path: PathBuf::from("/etc/app.toml"),
        };
        assert_eq!(err.to_string(), "config file not found: /etc/app.toml");
    }

    #[test]
    fn test_display_validation() {
        let err = ErrorName::Validation {
            field: "email".to_string(),
            message: "must contain @".to_string(),
        };
        assert_eq!(err.to_string(), "email: must contain @");
    }

    #[test]
    fn test_from_io_error() {
        let io_err = std::io::Error::new(std::io::ErrorKind::NotFound, "gone");
        let err = ErrorName::from(io_err);
        assert!(matches!(err, ErrorName::Io(_)));
    }

    #[test]
    fn test_source_chain() {
        let io_err = std::io::Error::new(std::io::ErrorKind::Other, "disk full");
        let err = ErrorName::from(io_err);
        assert!(std::error::Error::source(&err).is_some());
    }

    #[test]
    fn test_error_is_send_sync() {
        fn assert_send_sync<T: Send + Sync + 'static>() {}
        assert_send_sync::<ErrorName>();
    }
}
```

### Test Guidelines

- Test `Display` output for every variant — error messages are part of the user experience.
- Test `From` conversions for every source error type.
- Test `source()` returns the inner error for variants that wrap one.
- Include the `Send + Sync + 'static` compile-time assertion — this catches issues early.
- For `anyhow` usage in binaries, test that functions return `Err` with the expected context string using `format!("{:#}", err)` to see the full chain.
