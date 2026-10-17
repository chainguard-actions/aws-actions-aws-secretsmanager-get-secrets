<!-- markdownlint-disable -->

# Hardening Report: aws-actions--aws-secretsmanager-get-secrets/v3.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **aws-actions--aws-secretsmanager-get-secrets/v3.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a) violation: The composite action's 'Determine role to assume' step directly interpolates `${{ github.event_name }}` inside a `run:` shell command string. Before the shell ever sees the command, GitHub Actions performs YAML template substitution, injecting the raw value of `github.event_name` into the shell script. An attacker who can control this context value (e.g. via a crafted event name) could inject arbitrary shell commands. The offending line is: `if [ "${{ github.event_name }}" == "push" ]; then`. The fix is to pass the value via an `env:` variable and reference it as `"$GITHUB_EVENT_NAME"` (or the pre-set `$GITHUB_EVENT_NAME` environment variable) instead.

Locations:

- `.github/actions/build/action.yml:20`

### unpinned-uses (severity: high)

The composite action references `aws-actions/configure-aws-credentials@v6`, which uses a mutable tag (`v6`) rather than a full 40-character immutable commit SHA. If the tag is moved (intentionally or via a supply-chain attack), the action will silently execute different code. Pin to a specific commit SHA, e.g. `aws-actions/configure-aws-credentials@<40-char-sha> # v6`.

Locations:

- `.github/actions/build/action.yml:27`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

Fixed two findings in hardened/action/.github/actions/build/action.yml: (1) script-injection: moved `${{ github.event_name }}` from the `run:` shell string into an `env:` block as `EVENT_NAME`, referencing it as `$EVENT_NAME` in the shell script; (2) unpinned-uses: pinned `aws-actions/configure-aws-credentials@v6` to the full commit SHA `e1253824e5c10ff9df46874f81ed3ec929e19cfd` with the tag preserved as a comment.

