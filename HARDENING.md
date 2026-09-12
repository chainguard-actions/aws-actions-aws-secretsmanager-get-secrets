<!-- markdownlint-disable -->

# Hardening Report: aws-actions--aws-secretsmanager-get-secrets/v3.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **aws-actions--aws-secretsmanager-get-secrets/v3.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The `run:` block in the 'Determine role to assume' step directly interpolates `${{ github.event_name }}` inside a shell command string: `if [ "${{ github.event_name }}" == "push" ]; then`. GitHub Actions performs YAML template substitution before the shell ever sees the string, so an attacker-controlled or unexpected value could break out of the quoted context. The safe fix is to pass the value via an `env:` variable and reference it as `"$EVENT_NAME"` in the shell script.

Locations:

- `.github/actions/build/action.yml:20`

### unpinned-uses (severity: high)

The composite action references `aws-actions/configure-aws-credentials@v6`, which uses a mutable version tag rather than a full 40-character commit SHA. A tag can be moved to point to a different (potentially malicious) commit at any time, making this a supply-chain risk. Pin to a specific SHA, e.g. `aws-actions/configure-aws-credentials@e3dd6a429d7300a6a4c196c26e071d42e0343502 # v6`.

Locations:

- `.github/actions/build/action.yml:28`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

Fixed two findings in hardened/action/.github/actions/build/action.yml: (1) script-injection: moved `${{ github.event_name }}` into an `env:` block as `EVENT_NAME` and updated the shell script to use `$EVENT_NAME` instead of the direct template expression; (2) unpinned-uses: pinned `aws-actions/configure-aws-credentials@v6` to the full commit SHA `cbe3b392738ccf3f987d68400dafcf4b0624a56c` with a `# v6` comment.

