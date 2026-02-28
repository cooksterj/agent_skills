---
name: jira-ticket
description: Generate standardized Jira tickets (epic, story, task, sub-task) with consistent sections for Description, Acceptance Criteria, Testing, and Definition of Done. Use when creating Jira tickets or writing ticket content.
argument-hint: "[ticket-type] [brief summary]"
---

# Jira Ticket Generator

Generate a standardized Jira ticket using the template below. Every ticket must include all four sections regardless of ticket type.

## Input

- **Ticket type**: `$0` (must be one of: `epic`, `story`, `task`, `sub-task`)
- **Summary**: `$1` (brief summary of what the ticket is about)

If the ticket type or summary is missing, ask the user for the missing information before generating.

## Instructions

1. Determine the ticket type from the arguments. If not provided or not one of `epic`, `story`, `task`, `sub-task`, ask the user.
2. Use the summary to generate content for all four sections.
3. Tailor the depth and scope of each section to the ticket type:
   - **Epic**: High-level, broader scope, multiple deliverables
   - **Story**: User-focused, single feature or capability
   - **Task**: Technical work item, specific implementation detail
   - **Sub-task**: Narrow, granular piece of work under a parent task
4. Output the ticket using the exact format from the template below.
5. Present the generated ticket to the user for review.

## Template

See [TEMPLATE.md](TEMPLATE.md) for the standard ticket format.

## Formatting Rules

- Use standard markdown (the MCP Atlassian tool converts markdown to ADF for Jira)
- Use `##` for section headers
- Use `**bold**` for emphasis, `*italic*` for italics
- Use `1.` for ordered lists, `*` for unordered lists
- Use backtick `` ` `` for inline code and triple backtick ` ``` ` for code blocks
- Use markdown tables (`| Header |` with `|---|` separator row)
- Definition of Done items use plain `*` bullet list (MCP tool does not support checkbox syntax)
- Keep the Description to 1-2 sentences maximum
- Acceptance Criteria should have 3-6 bullet points
- Testing should map directly to Acceptance Criteria items
- Definition of Done items should focus on testing completion and verification
