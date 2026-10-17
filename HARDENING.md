<!-- markdownlint-disable -->

# Hardening Report: aws-actions--aws-secretsmanager-get-secrets/v2.0.10

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **aws-actions--aws-secretsmanager-get-secrets/v2.0.10** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A GitHub Actions expression is directly interpolated inside a `run:` shell command string. In `.github/actions/build/action.yml` line 20, `${{ github.repository_owner }}` is embedded directly in the shell `if` condition: `if [ "${{ github.repository_owner }}" == "aws-actions" ]; then`. The `github.repository_owner` value flows through YAML template substitution before the shell ever sees it, allowing an attacker to inject shell metacharacters. The fix is to pass the value via an `env:` variable and reference it as a quoted shell variable (e.g., `env: REPO_OWNER: ${{ github.repository_owner }}` then `if [ "$REPO_OWNER" == "aws-actions" ]; then`).

Locations:

- `.github/actions/build/action.yml:20`

### unpinned-uses (severity: high)

The composite action step `Configure AWS Credentials` references `aws-actions/configure-aws-credentials@v4`, which uses a mutable tag (`@v4`) rather than a full 40-character commit SHA. A tag can be moved to point to a different (potentially malicious) commit at any time, creating a supply-chain risk. It should be pinned to a specific commit SHA, e.g., `aws-actions/configure-aws-credentials@e3dd6a429d7300a6a4c196c26e831c8b509e6a38 # v4`.

Locations:

- `.github/actions/build/action.yml:29`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

Fixed two findings in hardened/action/.github/actions/build/action.yml: (1) script-injection: moved `${{ github.repository_owner }}` from the `run:` shell string into an `env:` block as `REPO_OWNER`, then referenced it as `"$REPO_OWNER"` in the shell condition; (2) unpinned-uses: pinned `aws-actions/configure-aws-credentials@v4` to the full commit SHA `7474bc4690e29a8392af63c5b98e7449536d5c3a` with `# v4` comment for readability.

