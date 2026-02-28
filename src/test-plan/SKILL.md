---
name: test-plan
description: Create practical test plans from code changes, mapping acceptance criteria to unit, integration, and end-to-end coverage including edge and failure cases.
argument-hint: "[scope]"
---

# Test Plan Generator

Create a structured test plan that can be executed by developers or QA before merge.

## Input

- **Scope**: `$0` (optional short scope label)

If scope is not provided, infer scope from changed files and behavior.

## Instructions

1. Inspect changes (`git diff` or provided files) and identify behavior changes.
2. Produce a test matrix that includes:
   - Unit tests
   - Integration tests
   - End-to-end/system tests (if applicable)
3. For each case, include:
   - Purpose
   - Preconditions / setup
   - Steps
   - Expected result
4. Include explicit edge and failure-path tests.
5. List any automation opportunities and exact command suggestions.
6. Add a short risk-based prioritization (P0/P1/P2).

## Output rules

- Prefer checklists and tables.
- Keep steps deterministic and verifiable.
- Avoid implementation detail unless needed for setup.

## Template

See [TEMPLATE.md](TEMPLATE.md).
