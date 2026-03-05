# Rust Scaffold Template

Use the section structure below when generating each scaffold type. Replace placeholder text with content derived from the user's description and the project's conventions.

---

## Common Structure (all types)

### Module Documentation

```rust
//! One-line summary of the module's purpose.
//!
//! Extended description if the module warrants it, otherwise omit.
```

### Imports

Only include `use` statements actually referenced in the generated code. Order: `std`, blank line, external crates, blank line, crate-local imports.

---

## struct

```rust
/// One-line summary of the struct's purpose.
///
/// Extended description if needed.
#[derive(Debug, Clone, PartialEq, Eq)]
pub struct StructName {
    /// Description of this field.
    field_name: FieldType,
    /// Description of this field.
    optional_field: Option<FieldType>,
}

impl StructName {
    /// Create a new `StructName`.
    ///
    /// # Errors
    ///
    /// Returns `ErrorType` if validation fails.
    pub fn new(field_name: impl Into<String>) -> Result<Self, ErrorType> {
        todo!()
    }

    /// One-line summary of the method.
    pub fn method_name(&self) -> ReturnType {
        todo!()
    }
}

impl fmt::Display for StructName {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        todo!()
    }
}
```

### Guidelines

- Always derive `Debug`. Add `Clone`, `PartialEq`, `Eq` when the type supports them.
- Include a `new()` constructor that validates inputs and returns `Result` when construction can fail.
- Use `impl Into<String>` for constructor parameters stored as `String`.
- Implement `Display` when the type has a natural text representation.
- Implement `Default` when there are sensible defaults for all fields.
- Stub methods with `todo!()`.
- Keep fields private by default. Provide accessor methods if read access is needed externally.

---

## enum

```rust
/// One-line summary of the enum's purpose.
#[derive(Debug, Clone, Copy, PartialEq, Eq)]
#[non_exhaustive]
pub enum EnumName {
    /// Description of this variant.
    VariantA,
    /// Description of this variant.
    VariantB(InnerType),
    /// Description of this variant.
    VariantC { field: FieldType },
}

impl EnumName {
    /// Returns `true` if the value is [`VariantA`](Self::VariantA).
    #[must_use]
    pub fn is_variant_a(&self) -> bool {
        matches!(self, Self::VariantA)
    }

    /// One-line summary of the method.
    pub fn method_name(&self) -> ReturnType {
        match self {
            Self::VariantA => todo!(),
            Self::VariantB(inner) => todo!(),
            Self::VariantC { field } => todo!(),
        }
    }
}

impl fmt::Display for EnumName {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        match self {
            Self::VariantA => write!(f, "VariantA"),
            Self::VariantB(inner) => write!(f, "{inner}"),
            Self::VariantC { field } => write!(f, "{field}"),
        }
    }
}
```

### Guidelines

- Use `#[non_exhaustive]` on public enums to allow adding variants without a breaking change.
- Derive `Copy` only when all variants are simple (no heap-allocated data).
- Implement `Display` for enums that represent user-facing values or error categories.
- Use `matches!` macro for simple boolean variant checks.
- Exhaustive `match` is preferred over `if let` when the enum has multiple variants.
- Include `is_*` convenience methods for commonly checked variants.

---

## trait

```rust
/// One-line summary of the trait's purpose.
///
/// Extended description of the contract this trait defines.
pub trait TraitName {
    /// The associated type description.
    type Output;

    /// One-line summary of the required method.
    ///
    /// # Errors
    ///
    /// Returns an error if the operation fails.
    fn required_method(&self) -> Result<Self::Output, ErrorType>;

    /// One-line summary of the provided method.
    ///
    /// Default implementation description.
    fn provided_method(&self) -> ReturnType {
        todo!()
    }
}
```

### Guidelines

- Keep traits focused — prefer small, composable traits over large monolithic ones.
- Use associated types (`type Output`) when the implementing type determines the return type.
- Use generics on the trait (`TraitName<T>`) when multiple implementations for the same type with different type parameters are expected.
- Provide default implementations for methods that have a natural "obvious" behavior.
- Mark required methods as `fn` with no body (just the signature and a semicolon).
- Include `# Errors` in rustdoc when trait methods return `Result`.
- If the trait should be object-safe, avoid generic methods, `Self` in return position, and associated constants/types with bounds not satisfied by `dyn Trait`.

---

## function

```rust
/// One-line summary of the function's purpose.
///
/// Extended description if the function's behavior is non-obvious.
///
/// # Errors
///
/// Returns `ErrorType` when the operation fails.
#[must_use]
pub fn function_name(param_name: &ParamType) -> Result<ReturnType, ErrorType> {
    todo!()
}

/// Helper for `function_name` that handles a specific sub-task.
fn helper_name(param: &ParamType) -> HelperReturnType {
    todo!()
}
```

### Guidelines

- Use early-return guard clauses for preconditions.
- Prefer returning `Result` over panicking. Use `panic!` only for programmer errors (invariant violations).
- Add `#[must_use]` on pure functions whose return value should not be silently discarded.
- Prefer `&T` parameters over owned `T` unless the function needs ownership.
- Keep helper functions private and place them below the public function they support.
- Stub bodies with `todo!()`.

---

## module

For a `module` scaffold, generate a directory module with the following structure:

```
module_name/
├── mod.rs          # Public API re-exports and module docs
├── types.rs        # Core types (structs, enums)
├── traits.rs       # Trait definitions (if applicable)
└── tests.rs        # Shared test utilities (if needed beyond inline tests)
```

### mod.rs

```rust
//! One-line summary of the module's purpose.
//!
//! Extended description of the module's responsibilities and how its
//! components fit together.

mod types;
mod traits;

pub use types::MainType;
pub use traits::MainTrait;
```

### Guidelines

- Only split into sub-modules when the module genuinely has distinct logical groupings.
- For simple modules, a single file is fine — do not force the directory structure.
- Re-export the primary public types from `mod.rs` so consumers can import from the module root.
- Keep `mod` declarations and `pub use` re-exports at the top of `mod.rs`.
- Place `#[cfg(test)]` modules inline in each sub-module file, not in a shared `tests.rs` unless test utilities are reused across sub-modules.

---

## Test Module (all types)

Generate a `#[cfg(test)]` module at the bottom of the scaffold file.

```rust
#[cfg(test)]
mod tests {
    use super::*;

    /// Helper to create a test instance with defaults.
    fn make_test_instance() -> StructName {
        StructName::new("test_value").expect("test instance should be valid")
    }

    #[test]
    fn test_new_with_valid_input() {
        let instance = StructName::new("valid").expect("should succeed");
        assert_eq!(instance.field_name(), "valid");
    }

    #[test]
    fn test_new_with_invalid_input_returns_error() {
        let result = StructName::new("");
        assert!(result.is_err());
    }

    #[test]
    fn test_method_name_basic_behavior() {
        let instance = make_test_instance();
        let result = instance.method_name();
        assert_eq!(result, expected_value);
    }

    #[test]
    fn test_display_formatting() {
        let instance = make_test_instance();
        assert_eq!(format!("{instance}"), "expected string");
    }

    #[test]
    fn test_enum_variant_matching() {
        let value = EnumName::VariantA;
        assert!(value.is_variant_a());
        assert!(matches!(value, EnumName::VariantA));
    }

    #[test]
    fn test_trait_impl_with_mock() {
        struct MockImpl;
        impl TraitName for MockImpl {
            type Output = String;
            fn required_method(&self) -> Result<Self::Output, ErrorType> {
                Ok("mock".to_string())
            }
        }

        let mock = MockImpl;
        assert_eq!(mock.required_method().unwrap(), "mock");
    }
}
```

### Test Guidelines

- One `#[cfg(test)] mod tests` per file, placed at the bottom.
- Test function names: `test_<function_or_method>_<scenario>` in `snake_case`.
- Test every public method, constructor, variant, `Display` impl, and `Result::Err` path.
- Include edge case tests: empty inputs, boundary values, `None` handling.
- For traits, define a minimal mock/stub struct inside the test module.
- For enums, test every variant and every branch in `match` methods.
- Use helper functions (e.g., `make_test_instance()`) to reduce test boilerplate.
- Prefer `assert_eq!` for value comparisons, `assert!(matches!(...))` for pattern matching.
- Use `#[should_panic(expected = "message")]` for expected panics.
- Use `-> Result<(), Box<dyn std::error::Error>>` with `?` in tests that exercise fallible code paths for cleaner error reporting.
- Match the project's existing test patterns. Default to inline `#[cfg(test)]` when no convention exists.
