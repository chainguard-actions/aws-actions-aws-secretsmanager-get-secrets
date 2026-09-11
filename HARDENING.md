<!-- markdownlint-disable -->

# Hardening Report: aws-actions--aws-secretsmanager-get-secrets/v2.0.10

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **aws-actions--aws-secretsmanager-get-secrets/v2.0.10** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A GitHub Actions expression `${{ github.repository_owner }}` is directly interpolated inside a `run:` shell command string. Even though `github.repository_owner` is typically GitHub-controlled, any `${{ ... }}` expression inside a `run:` block undergoes YAML template substitution before the shell sees it, enabling script injection. The offending line is: `if [ "${{ github.repository_owner }}" == "aws-actions" ]; then`. It should be moved to an `env:` variable and referenced as a quoted shell variable instead.

Locations:

- `.github/actions/build/action.yml:19`

### unpinned-uses (severity: high)

The composite action references `aws-actions/configure-aws-credentials@v4`, which uses a mutable version tag (`@v4`) rather than a full 40-character commit SHA. A mutable tag can be silently redirected to a different (potentially malicious) commit, enabling supply-chain attacks. Pin to a specific SHA, e.g. `aws-actions/configure-aws-credentials@e3dd6a429d7300a6a4c196c26e071d42e0343502 # v4`.

Locations:

- `.github/actions/build/action.yml:27`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

Fixed two findings in hardened/action/.github/actions/build/action.yml: (1) script-injection: moved `${{ github.repository_owner }}` out of the `run:` shell string into an `env:` block as `REPOSITORY_OWNER`, referencing it as `"$REPOSITORY_OWNER"` in the shell script; (2) unpinned-uses: pinned `aws-actions/configure-aws-credentials@v4` to its full commit SHA `7474bc4690e29a8392af63c5b98e7449536d5c3a` with `# v4` comment for readability.

