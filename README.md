# gh-aw checkbox smoke test

Minimal public repro for testing whether a Copilot-based `gh-aw` workflow can run from a PR checkbox menu edited by a human user.

## What this repo tests

- `gh-aw` workflow source in `.github/workflows/gh-aw-skill-smoke.md`.
- `engine: copilot`.
- A docs-content-style PR checkbox menu in `.github/workflows/pr-checkbox-menu.yml`.
- A manual `issue_comment(edited)` trigger where `github.actor` is the human editor and `github.event.comment.user.login` is `github-actions[bot]`.
- An `aw_context` value with `allow-bot-authored-trigger-comment` set to `true`.

## Expected behavior

When a maintainer edits the bot-authored PR menu comment and checks **Run checkbox AW smoke**, the menu workflow should:

1. Detect the unchecked-to-checked transition.
2. Call the reusable `gh-aw` smoke workflow.
3. Let the agent post a comment that starts with `Checkbox AW smoke reached agent.`

The workflow intentionally avoids APM and skill imports so the result focuses on the checkbox-menu confused-deputy path.

## Run it

Before triggering the workflow, configure the `COPILOT_GITHUB_TOKEN` repository secret.

The checkbox menu workflow must be present on the default branch for `issue_comment(edited)` events to run.

This test PR exists only to exercise the checkbox menu flow.

1. Open a pull request that changes any file.
2. Run **PR checkbox menu smoke** with `workflow_dispatch` and the pull request number to post the menu comment.
3. Edit the bot-authored menu comment, change `- [ ] Run checkbox AW smoke` to `- [x] Run checkbox AW smoke`, and save.
4. Confirm the `Checkbox smoke AW` job runs and the agent posts the smoke comment.

