<!-- markdownlint-disable -->

# Hardening Report: aws-actions--aws-secretsmanager-get-secrets/v3.0.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **aws-actions--aws-secretsmanager-get-secrets/v3.0.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A GitHub Actions expression is directly interpolated inside a `run:` shell command string. On line 20 of `.github/actions/build/action.yml`, the value `${{ github.event_name }}` is embedded directly in the shell `if` condition: `if [ "${{ github.event_name }}" == "push" ]`. This causes the expression to be substituted into the shell script before the shell parses it, allowing an attacker who can control the event name (e.g. via a crafted workflow dispatch or repository fork) to inject arbitrary shell commands. The fix is to pass the value via an `env:` variable and reference it as a quoted shell variable: `if [ "$GITHUB_EVENT_NAME" == "push" ]`.

Locations:

- `.github/actions/build/action.yml:20`

### unpinned-uses (severity: high)

The composite action step `uses: aws-actions/configure-aws-credentials@v6` references a mutable tag (`v6`) rather than a full 40-character commit SHA. If the tag is moved (intentionally or via a supply-chain compromise), the action will silently execute different code. Pin to a specific commit SHA, e.g. `uses: aws-actions/configure-aws-credentials@e3dd6a429d7300a6a4c196c26e071d42e0343502 # v6`.

Locations:

- `.github/actions/build/action.yml:28`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

Fixed two findings in hardened/action/.github/actions/build/action.yml: (1) script-injection: moved `${{ github.event_name }}` from the `run:` shell string into an `env:` block as `GITHUB_EVENT_NAME`, referencing it as `$GITHUB_EVENT_NAME` in the shell condition; (2) unpinned-uses: pinned `aws-actions/configure-aws-credentials@v6` to the full commit SHA `cbe3b392738ccf3f987d68400dafcf4b0624a56c` with a `# v6` comment.

