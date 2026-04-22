---
description: |
  Minimal smoke test for APM-restored skill invocation under Copilot.

inlined-imports: true
imports:
  - uses: github/gh-aw/.github/workflows/shared/apm.md@v0.69.0
    with:
      packages:
        - elastic/elastic-docs-skills/skills/review/docs-check-style
engine:
  id: copilot
on:
  workflow_dispatch:
  roles: [admin, maintainer, write]
permissions:
  contents: read
mcp-servers:
  elastic-docs:
    type: http
    url: "https://www.elastic.co/docs/_mcp/"
    allowed: ["*"]
network:
  allowed:
    - defaults
    - github
    - "www.elastic.co"
safe-outputs:
  noop:
timeout-minutes: 15
---

# Skill smoke test

You are testing whether an APM-restored skill is invocable under the Copilot engine.

The imported skill must be invoked using this exact form:

- `skill(skill: docs-check-style)`

Do not guess another invocation format.

## Task

1. Read `README.md`.
2. Attempt to invoke `skill(skill: docs-check-style)`.
3. If the skill invocation succeeds, call `noop` with a short message that includes all of the following exact values returned by the skill:
   - version: `1.0.4`
   - allowed-tools: `Read, Grep, Glob, Bash(vale *), WebFetch`
   - first source URL: `https://www.elastic.co/docs/contribute-docs/style-guide`
4. If you cannot confirm all three exact values from the invoked skill output, treat the invocation as unverified and say so in the `noop` message.
5. If the skill invocation fails, call `noop` with the exact failure text and state which invocation form you actually attempted.
6. Do not perform any fallback review. This workflow is only a skill invocation test.
