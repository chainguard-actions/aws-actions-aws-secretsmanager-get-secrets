<!-- markdownlint-disable -->

# Hardening Report: aws-actions--aws-secretsmanager-get-secrets/v3.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **aws-actions--aws-secretsmanager-get-secrets/v3.0.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a) violation: The composite action directly interpolates `${{ github.event_name }}` inside a `run:` shell block. The Actions template engine substitutes this value before the shell parses the command, allowing an attacker who can control the event name to inject arbitrary shell commands. The offending line is: `if [ "${{ github.event_name }}" == "push" ]; then`. This should be replaced with the environment variable `$GITHUB_EVENT_NAME` (already available in the runner environment) and referenced as a quoted shell variable instead.

Locations:

- `.github/actions/build/action.yml:20`

### unpinned-uses (severity: high)

The composite action references `aws-actions/configure-aws-credentials@v6`, which uses a mutable version tag rather than a pinned 40-character commit SHA. A tag can be moved to point to a different (potentially malicious) commit at any time, making this a supply-chain risk. It should be pinned to a full SHA, e.g. `aws-actions/configure-aws-credentials@<40-char-sha> # v6`.

Locations:

- `.github/actions/build/action.yml:29`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

Fixed two findings in hardened/action/.github/actions/build/action.yml: (1) script-injection — replaced `${{ github.event_name }}` with the shell environment variable `$GITHUB_EVENT_NAME`, which is already provided by the GitHub Actions runner, eliminating the template-engine injection risk; (2) unpinned-uses — pinned `aws-actions/configure-aws-credentials@v6` to the full commit SHA `e1253824e5c10ff9df46874f81ed3ec929e19cfd # v6`.

