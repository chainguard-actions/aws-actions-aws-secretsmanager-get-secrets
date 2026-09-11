<!-- markdownlint-disable -->

# Hardening Report: aws-actions--aws-secretsmanager-get-secrets/v3.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **aws-actions--aws-secretsmanager-get-secrets/v3.0.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The `${{ github.event_name }}` expression is directly interpolated inside a `run:` shell command string in the composite action. This causes the GitHub Actions expression to be substituted into the shell script before execution, which is a script-injection risk. The offending line is: `if [ "${{ github.event_name }}" == "push" ]; then`. It should be replaced with an environment variable approach: set `EVENT_NAME: ${{ github.event_name }}` in an `env:` block and reference `"$EVENT_NAME"` in the shell script.

Locations:

- `.github/actions/build/action.yml:18`

### unpinned-uses (severity: high)

The composite action uses `aws-actions/configure-aws-credentials@v6`, which is pinned to a mutable tag (`@v6`) rather than an immutable 40-character commit SHA. A supply-chain attacker could push a new commit to that tag and inject malicious code. It should be pinned to a full SHA, e.g. `aws-actions/configure-aws-credentials@<40-char-sha> # v6`.

Locations:

- `.github/actions/build/action.yml:27`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

Fixed two findings in hardened/action/.github/actions/build/action.yml: (1) script-injection: moved `${{ github.event_name }}` from the run: shell script into an env: block as EVENT_NAME, then referenced $EVENT_NAME in the shell script to prevent expression interpolation before shell execution; (2) unpinned-uses: pinned aws-actions/configure-aws-credentials from the mutable @v6 tag to the full commit SHA @cbe3b392738ccf3f987d68400dafcf4b0624a56c with # v6 comment for readability.

