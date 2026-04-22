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
3. If the skill invocation succeeds, call `noop` with a short message saying the skill was invocable.
4. If the skill invocation fails, call `noop` with the exact failure text and state which invocation form you actually attempted.
5. Do not perform any fallback review. This workflow is only a skill invocation test.
