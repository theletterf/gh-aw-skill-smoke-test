---
description: |
  Minimal smoke test for APM-restored skill invocation under Copilot.

inlined-imports: true
imports:
  - uses: github/gh-aw/.github/workflows/shared/apm.md@v0.70.0
    with:
      packages:
        - elastic/elastic-docs-skills/skills/review/docs-check-style
engine:
  id: copilot
on:
  pull_request:
    types: [opened, synchronize, reopened]
  workflow_dispatch:
  roles: [admin, maintainer, write]
permissions:
  contents: read
  pull-requests: read
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
  add-comment:
    hide-older-comments: true
  noop:
timeout-minutes: 15
---

# Skill smoke test

You are testing whether an APM-restored skill is invocable under the Copilot engine while reviewing a pull request that changes markdown under `docs/`.

The imported skill must be invoked using this exact form:

- `skill(skill: docs-check-style)`

Do not guess another invocation format.

## Task

1. If this run is in pull request context, inspect the changed files and focus only on changed markdown files under `docs/`.
2. If there are no changed `docs/**/*.md` files, call `noop` and say so.
3. Attempt to invoke `skill(skill: docs-check-style)`.
4. If the skill invocation succeeds, review the changed markdown file with that skill and post one concise PR comment using `add-comment`.
5. The PR comment must include all of the following exact values returned by the skill:
   - version: `1.0.5`
   - allowed-tools: `Read, Grep, Glob, Bash(vale *), WebFetch`
   - first source URL: `https://www.elastic.co/docs/contribute-docs/style-guide`
6. If the skill invocation fails, call `add-comment` with the exact failure text and state which invocation form you actually attempted.
7. Do not perform a fallback manual review. This workflow is only a skill invocation and skill-driven review test.
