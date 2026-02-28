# Test Plan Template

```md
## Scope
- [Feature / bugfix area]

## Risks
- P0: [Critical paths]
- P1: [Important but non-blocking]
- P2: [Nice-to-have coverage]

## Test Matrix
| Priority | Level | Scenario | Setup | Steps | Expected |
|---|---|---|---|---|---|
| P0 | Unit | [Scenario] | [Setup] | [Steps] | [Expected] |
| P0 | Integration | [Scenario] | [Setup] | [Steps] | [Expected] |
| P1 | E2E | [Scenario] | [Setup] | [Steps] | [Expected] |

## Edge and failure cases
- [Boundary input case]
- [Invalid input / error handling]
- [Timeout / dependency failure]

## Automation
- Command: `[exact test command]`
- Coverage target: [goal]
- Gaps to automate later: [list]
```
