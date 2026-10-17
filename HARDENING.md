<!-- markdownlint-disable -->

# Hardening Report: aws-actions--aws-secretsmanager-get-secrets/v3.0.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **aws-actions--aws-secretsmanager-get-secrets/v3.0.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A GitHub Actions expression `${{ github.event_name }}` is directly interpolated inside a `run:` shell command string. Before the shell executes the script, GitHub Actions substitutes the expression value as raw text, which can allow injection of shell metacharacters. The offending line is: `if [ "${{ github.event_name }}" == "push" ]; then`. This should be replaced with an environment variable reference, e.g. set `EVENT_NAME: ${{ github.event_name }}` in an `env:` block and use `"$EVENT_NAME"` in the shell script.

Locations:

- `.github/actions/build/action.yml:19`

### unpinned-uses (severity: high)

The step `uses: aws-actions/configure-aws-credentials@v6` references a mutable tag (`@v6`) rather than a full 40-character commit SHA. A tag can be moved to point to a different (potentially malicious) commit at any time, creating a supply-chain risk. Pin to a specific SHA, e.g. `aws-actions/configure-aws-credentials@e3dd6a429d7300a6a4c196c26e071d42e0343502 # v6`.

Locations:

- `.github/actions/build/action.yml:28`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

Fixed both findings in .github/actions/build/action.yml: (1) Moved `${{ github.event_name }}` into an `env:` block as `EVENT_NAME` and replaced the inline expression with `"$EVENT_NAME"` in the shell script to prevent script injection. (2) Pinned `aws-actions/configure-aws-credentials@v6` to its full commit SHA `e1253824e5c10ff9df46874f81ed3ec929e19cfd # v6`.

