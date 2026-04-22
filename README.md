# gh-aw skill smoke test

Minimal public repro for testing whether a Copilot-based `gh-aw` workflow can invoke an APM-restored skill.

## What this repo tests

- `gh-aw` workflow source in `.github/workflows/gh-aw-skill-smoke.md`.
- `engine: copilot`.
- APM import of one public Elastic Docs Skill: `docs-check-style`.
- A minimal prompt that attempts exactly one skill invocation and reports whether it worked.

## Expected behavior

When the workflow runs through `workflow_dispatch`, it should:

1. Read this `README.md`.
2. Attempt to invoke `skill(skill: docs-check-style)`.
3. Report success or failure through `noop`.

The workflow intentionally avoids any fallback review logic so the result focuses on skill invocation only.

## Run it

Before triggering the workflow, configure the `COPILOT_GITHUB_TOKEN` repository secret.

Then run the smoke test from the Actions tab by using `workflow_dispatch` on `Skill smoke test`.

The workflow should:

1. Read `README.md`.
2. Attempt `skill(skill: docs-check-style)`.
3. Return a `noop` result that says whether the skill invocation succeeded or failed, without falling back to a manual review.
