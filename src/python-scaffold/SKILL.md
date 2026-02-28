---
name: python-scaffold
description: Generate idiomatic Python classes, dataclasses, functions, or ABCs with type hints, numpydoc docstrings, and complete test coverage that match the project's existing conventions. Use when scaffolding new Python code.
argument-hint: "[type] [brief description]"
---

# Python Scaffold Generator

Generate a minimal, idiomatic Python scaffold and its corresponding test file using the template below. Every scaffold must include type hints, numpydoc-format docstrings, and complete test coverage.

## Input

- **Scaffold type**: `$0` (must be one of: `class`, `dataclass`, `function`, `abc`)
- **Description**: `$1` (brief description of what the code should do)

If the scaffold type or description is missing, ask the user for the missing information before generating.

## Instructions

1. Determine the scaffold type from the arguments. If not provided or not one of `class`, `dataclass`, `function`, `abc`, ask the user.
2. Before generating, scan the target project for existing conventions:
   - Import style and ordering (stdlib, third-party, local)
   - Naming conventions (snake_case functions, PascalCase classes)
   - Use of `from __future__ import annotations`
   - Existing base classes, mixins, or patterns that the new code should follow
   - Project tooling (pyproject.toml, setup.cfg) for Python version targets
   - Test framework and patterns (pytest fixtures, test class organization, assertion style)
3. Generate the scaffold following the template structure, tailored to the scaffold type.
4. Generate a complete test file covering every public method, function, branch, and edge case.
5. Place both files where they logically fit within the existing project structure. If the location is ambiguous, ask the user.
6. Present the generated code and tests to the user for review.

## Docstring Rules

- Use numpydoc format for all docstrings (module, class, method, function).
- Do **not** include an `Examples` section unless the user explicitly requests examples.
- Every public method and function must have a docstring with at minimum a summary line.
- Include `Parameters`, `Returns`, and `Raises` sections only when applicable (omit empty sections).

## Structure Rules

- Do **not** define nested or inner functions (sub-functions). All functions and helpers must be defined at module level.
- If a piece of logic needs to be extracted, define it as a separate module-level function, not as a closure or nested def.

## Test Rules

- Generate a test file for every scaffold. Place it according to the project's existing test layout.
- Provide complete test coverage: every public method, function, property, classmethod, and code branch must have at least one test.
- Include tests for edge cases, error conditions, and `Raises` scenarios documented in docstrings.
- For ABCs, test concrete helper methods directly and create a minimal concrete subclass to verify the abstract interface.
- Match the project's existing test framework and patterns. Default to pytest when no convention exists.
- Test functions must also be module-level only (no nested helpers inside test functions).

## Generation Rules

- Produce a minimal skeleton the developer fills in. Do not generate speculative business logic.
- Only include imports that are actually used by the scaffold.
- Match the project's existing style. When no project context exists, default to modern Python 3.11+ idioms.
- Use `from __future__ import annotations` when the project targets Python < 3.10 or when the existing codebase uses it.

## Template

See [TEMPLATE.md](TEMPLATE.md) for the scaffold format by type.
