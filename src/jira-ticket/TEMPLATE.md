# Jira Ticket Template

Use this exact structure for every generated ticket. Replace placeholder text with content appropriate to the ticket type and summary.

---

## Description

Brief 1-2 sentence overview of what this ticket covers and why it is needed.

## Acceptance Criteria

* **AC-1:** [Detailed description of a specific requirement that must be met]
* **AC-2:** [Detailed description of a specific requirement that must be met]
* **AC-3:** [Detailed description of a specific requirement that must be met]

*Add more criteria as needed. Each criterion should be specific, measurable, and independently verifiable.*

## Testing

| Test Area | Expected Behavior | Relates To |
|---|---|---|
| [Area derived from AC-1] | [What should happen and how to verify it] | AC-1 |
| [Area derived from AC-2] | [What should happen and how to verify it] | AC-2 |
| [Area derived from AC-3] | [What should happen and how to verify it] | AC-3 |

**Additional testing considerations:**

* [Edge cases, negative tests, or integration scenarios to validate]
* [Performance, security, or accessibility checks if applicable]

## Definition of Done

* All acceptance criteria verified through testing
* Unit tests written and passing
* Integration/functional tests written and passing
* Edge cases and negative scenarios tested
* Code reviewed and approved
* No regressions introduced (existing tests passing)
* Documentation updated (if applicable)

---

## Section Guidelines by Ticket Type

### Epic
- **Description**: Broad initiative-level summary referencing the business goal
- **Acceptance Criteria**: High-level deliverables and success measures
- **Testing**: End-to-end validation strategy across the epic scope
- **Definition of Done**: Milestone-level items (all child stories complete, integration tested, stakeholder sign-off)

### Story
- **Description**: User-focused summary in the form "Enable [user] to [action] so that [outcome]"
- **Acceptance Criteria**: Specific user-facing behaviors and edge cases
- **Testing**: Functional test scenarios covering happy path and error states
- **Definition of Done**: Feature-level items (tests passing, demo-ready, acceptance from product)

### Task
- **Description**: Technical summary of the implementation work
- **Acceptance Criteria**: Technical requirements and implementation details
- **Testing**: Unit and integration test expectations
- **Definition of Done**: Implementation-level items (code complete, tests passing, reviewed)

### Sub-task
- **Description**: Narrow scope summary of the specific piece of work
- **Acceptance Criteria**: Granular requirements for this slice of work
- **Testing**: Targeted tests for this specific component
- **Definition of Done**: Granular items (specific tests passing, parent task unblocked)
