# Python Scaffold Template

Use the section structure below when generating each scaffold type. Replace placeholder text with content derived from the user's description and the project's conventions.

All functions must be defined at module level. Never nest a function inside another function, class method, or test.

---

## Common Structure (all types)

### Module Docstring

```python
"""One-line summary of the module's purpose.

Longer description if the module warrants it, otherwise omit.
"""
```

### Imports

Only include imports actually referenced in the generated code. Order: stdlib, blank line, third-party, blank line, local.

---

## class

```python
def _helper_for_class(param: type) -> return_type:
    """One-line summary of the helper's purpose.

    Class-specific helpers are module-level functions prefixed with ``_``
    to signal they are internal to this module and tightly coupled to
    the class defined here.

    Parameters
    ----------
    param : type
        Description of the parameter.

    Returns
    -------
    return_type
        Description of the return value.
    """
    raise NotImplementedError


class ClassName:
    """One-line summary.

    Extended description if needed.

    Parameters
    ----------
    param_name : type
        Description of the parameter.

    Attributes
    ----------
    attr_name : type
        Description of the attribute.
    """

    def __init__(self, param_name: type) -> None:
        self.attr_name = param_name

    def __repr__(self) -> str:
        return f"{type(self).__name__}({self.attr_name!r})"

    def method_name(self) -> return_type:
        """One-line summary.

        Parameters
        ----------
        (if applicable)

        Returns
        -------
        return_type
            Description of the return value.

        Raises
        ------
        ExceptionType
            When the exception is raised.
        """
        raise NotImplementedError
```

### Guidelines

- Include `__repr__` for debuggability.
- Stub key methods with `NotImplementedError`.
- Only include `__eq__` / `__hash__` if the class represents a value object.
- Do not define nested functions inside any method.
- Helper functions that serve a specific class must be defined at module level with a `_` prefix (e.g., `_parse_input`, `_validate_config`). This keeps them out of the public API while co-locating them with the class they support.
- Place `_`-prefixed helpers above the class they support so the class body can reference them.

---

## dataclass

```python
from dataclasses import dataclass


def _convert_source_field(raw_value: type) -> type:
    """One-line summary of the conversion logic.

    Dataclass-specific helpers are module-level functions prefixed with
    ``_`` to signal they are internal to this module.

    Parameters
    ----------
    raw_value : type
        Description of the raw input.

    Returns
    -------
    type
        Description of the converted value.
    """
    raise NotImplementedError


@dataclass(frozen=True)
class ClassName:
    """One-line summary.

    Parameters
    ----------
    field_name : type
        Description of the field.
    """

    field_name: type
    optional_field: type = default_value

    @classmethod
    def from_source(cls, raw: source_type) -> "ClassName":
        """Create an instance from an alternative source.

        Parameters
        ----------
        raw : source_type
            Description of the source input.

        Returns
        -------
        ClassName
            A new instance.
        """
        raise NotImplementedError
```

### Guidelines

- Default to `frozen=True` for immutability. Use mutable only when the user's description implies mutation.
- Include a factory classmethod when the data naturally comes from an external format (dict, row, JSON).
- Omit the factory classmethod when the dataclass is straightforward with no alternative construction path.
- Helper functions for dataclass construction or validation are module-level with a `_` prefix, placed above the dataclass definition.

---

## function

```python
def _helper_name(param: type) -> return_type:
    """One-line summary of the helper's purpose.

    Parameters
    ----------
    param : type
        Description of the parameter.

    Returns
    -------
    return_type
        Description of the return value.
    """
    raise NotImplementedError


def function_name(param_name: type) -> return_type:
    """One-line summary.

    Extended description if the function's behavior is non-obvious.

    Parameters
    ----------
    param_name : type
        Description of the parameter.

    Returns
    -------
    return_type
        Description of the return value.

    Raises
    ------
    ExceptionType
        When the exception is raised.
    """
    raise NotImplementedError
```

### Guidelines

- Use early-return guard clauses for preconditions.
- Prefer returning values over mutating arguments.
- Stub the body with `NotImplementedError`.
- If logic needs extraction, define a separate module-level function with a `_` prefix. Never nest a function inside another function.
- Place `_`-prefixed helpers above the public function that calls them.

---

## abc

```python
from abc import ABC, abstractmethod


def _shared_helper(param: type) -> return_type:
    """One-line summary of the helper's purpose.

    ABC-specific helpers are module-level functions prefixed with ``_``
    to signal they are internal to this module.

    Parameters
    ----------
    param : type
        Description of the parameter.

    Returns
    -------
    return_type
        Description of the return value.
    """
    raise NotImplementedError


class ClassName(ABC):
    """One-line summary.

    Extended description of the contract this ABC defines.
    """

    @abstractmethod
    def required_method(self) -> return_type:
        """One-line summary.

        Returns
        -------
        return_type
            Description of the return value.
        """
        ...

    def concrete_helper(self) -> return_type:
        """One-line summary.

        Shared implementation available to all subclasses.

        Returns
        -------
        return_type
            Description of the return value.
        """
        raise NotImplementedError
```

### Guidelines

- Use `@abstractmethod` for methods subclasses must implement.
- Use `...` (ellipsis) as the body for abstract methods.
- Include concrete helper methods only when there is clearly shared logic across implementations.
- Avoid abstract properties unless the user's description specifically calls for them.
- Module-level helpers shared across the ABC and its subclasses use a `_` prefix.

---

## Test File (all types)

Generate a test file for every scaffold. The test file must cover every public method, function, property, classmethod, and code branch. Tests must also cover `_`-prefixed helper functions.

```python
"""Tests for module_name."""

import pytest

from module_path import ClassName  # or function_name
from module_path import _helper_for_class  # test _ helpers directly


def _make_instance(**overrides: object) -> ClassName:
    """Create a ClassName with sensible defaults for testing.

    Test-specific helpers are also module-level with a ``_`` prefix.
    """
    defaults = {"param_name": "default_value"}
    defaults.update(overrides)
    return ClassName(**defaults)


class TestHelperForClass:
    """Tests for _helper_for_class."""

    def test_basic_behavior(self) -> None:
        """Verify helper produces expected output."""
        ...

    def test_edge_case(self) -> None:
        """Verify helper handles boundary input."""
        ...


class TestClassName:
    """Tests for ClassName."""

    def test_init(self) -> None:
        """Verify instance creation with valid parameters."""
        ...

    def test_repr(self) -> None:
        """Verify string representation."""
        ...

    def test_method_name(self) -> None:
        """Verify method_name behavior."""
        ...

    def test_method_name_raises_on_invalid_input(self) -> None:
        """Verify method_name raises ExceptionType for invalid input."""
        with pytest.raises(ExceptionType):
            ...

    def test_edge_case(self) -> None:
        """Verify behavior at boundary conditions."""
        ...
```

### Test Guidelines

- One test class per scaffold class, ABC, or `_`-prefixed helper. Use standalone test functions for scaffold functions.
- Every public method, classmethod, and property gets at least one test.
- Every `_`-prefixed helper function gets its own test class or test functions with direct coverage.
- Every `Raises` entry in a docstring gets a corresponding `pytest.raises` test.
- Include edge case tests: empty inputs, boundary values, None handling where applicable.
- For ABCs, define a minimal `_Concrete` subclass at module level in the test file (not nested) to verify the abstract interface.
- All test helpers must be module-level `_`-prefixed functions or pytest fixtures. Never nest functions inside test functions.
- Match the project's existing test patterns (fixtures, parametrize, assertion style). Default to pytest when no convention exists.
