<!-- markdownlint-disable -->

# Hardening Report: aws-actions--aws-secretsmanager-get-secrets/v3.0.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **aws-actions--aws-secretsmanager-get-secrets/v3.0.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a) violation: The `run:` block in the 'Determine role to assume' step directly interpolates `${{ github.event_name }}` inside a shell command string. This allows an attacker to inject arbitrary shell commands by controlling the event name (e.g., via a crafted workflow_dispatch or pull_request_target event). The offending line is: `if [ "${{ github.event_name }}" == "push" ]; then`. The value should be passed via an `env:` variable and then referenced as a quoted shell variable (e.g., `"$EVENT_NAME"`) instead.

Locations:

- `.github/actions/build/action.yml:20`

### unpinned-uses (severity: high)

The composite action uses `aws-actions/configure-aws-credentials@v6`, which is pinned to a mutable version tag rather than an immutable 40-character commit SHA. If the tag is moved (e.g., by a supply-chain compromise), the action will silently execute different code. It should be pinned to a full SHA, e.g., `aws-actions/configure-aws-credentials@<40-char-sha> # v6`.

Locations:

- `.github/actions/build/action.yml:29`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

Fixed two findings in .github/actions/build/action.yml: (1) script-injection: moved `${{ github.event_name }}` into an `env:` block as `EVENT_NAME` and referenced it as `"$EVENT_NAME"` in the shell script to prevent shell command injection; (2) unpinned-uses: pinned `aws-actions/configure-aws-credentials@v6` to its full commit SHA `cbe3b392738ccf3f987d68400dafcf4b0624a56c` with the tag preserved as a comment.

