---
description: |
  Minimal smoke test for PR checkbox-menu invocation under Copilot.

inlined-imports: true
engine:
  id: copilot
on:
  roles: [admin, maintainer, write]
  workflow_call:
    inputs:
      aw_context:
        description: "Agent caller context (used internally by Agentic Workflows)"
        type: string
        required: false
        default: ""
      smoke-label:
        description: "Label included in the smoke-test comment"
        type: string
        required: false
        default: "checkbox-menu"
    secrets:
      COPILOT_GITHUB_TOKEN:
        required: true
permissions:
  contents: read
  issues: read
  pull-requests: read
network:
  allowed:
    - defaults
    - github
safe-outputs:
  add-comment:
    hide-older-comments: true
  noop:
timeout-minutes: 15
---

# Checkbox menu smoke test

You are testing whether a PR checkbox menu can call this reusable Agentic Workflow from an `issue_comment(edited)` event.

## Task

Call `add-comment` with one concise comment that includes these exact lines:

- `Checkbox AW smoke reached agent.`
- `label: ${{ inputs.smoke-label }}`
- `event: ${{ github.event_name }}`
- `actor: ${{ github.actor }}`
- `issue-number: ${{ github.event.issue.number }}`
- `comment-id: ${{ github.event.comment.id }}`
