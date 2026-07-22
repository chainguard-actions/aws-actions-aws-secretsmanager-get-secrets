<!-- markdownlint-disable -->

# Hardening Report: aws-actions--aws-secretsmanager-get-secrets/v2.0.10

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **aws-actions--aws-secretsmanager-get-secrets/v2.0.10** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references across workflow files and the composite build action use mutable tags instead of pinned 40-character SHA digests, making the action vulnerable to supply-chain attacks if the referenced tag is moved.

Failing references:
- `.github/workflows/tests.yml`: `actions/checkout@v4`, `codecov/codecov-action@v5.4.2`
- `.github/workflows/package.yml`: `actions/checkout@v4`
- `.github/workflows/release.yml`: `actions/checkout@v4`
- `.github/actions/build/action.yml`: `aws-actions/configure-aws-credentials@v4`

Locations:

- `.github/workflows/tests.yml:17`
- `.github/workflows/tests.yml:22`
- `.github/workflows/package.yml:12`
- `.github/workflows/release.yml:7`
- `.github/actions/build/action.yml:22`

### script-injection (severity: high)

Sub-rule (a): In `.github/actions/build/action.yml`, the expression `${{ github.repository_owner }}` is directly interpolated inside a `run:` shell block. Before the shell executes, GitHub Actions substitutes the expression value verbatim into the script string, allowing an attacker who controls the repository owner name (e.g. via a fork) to inject arbitrary shell commands.

Offending line:
```
if [ "${{ github.repository_owner }}" == "aws-actions" ]; then
```

Locations:

- `.github/actions/build/action.yml:16`

### github-env-injection (severity: high)

In `.github/actions/build/action.yml`, the `run:` block writes to `$GITHUB_OUTPUT` inside a branch conditioned on `${{ github.repository_owner }}`, which is directly interpolated into the shell script without sanitization. The value written to GITHUB_OUTPUT (`arn=arn:aws:iam::...`) is a literal, but the unsanitized `${{ github.repository_owner }}` expression in the same run block means the entire step is vulnerable to injection. Additionally, the output value flows from this step into a subsequent `uses:` step via `steps.role-to-assume.outputs.arn` without any newline sanitization applied before the `echo ... >> "$GITHUB_OUTPUT"` write.

Offending step writes to $GITHUB_OUTPUT:
```
echo "arn=arn:aws:iam::339713045997:role/GithubActionsRole" >> "$GITHUB_OUTPUT"
```
The controlling condition uses the unsanitized `${{ github.repository_owner }}` expression.

Locations:

- `.github/actions/build/action.yml:18`

### missing-permissions (severity: medium)

The following workflow files have no top-level `permissions:` key and no job-level `permissions:` key on any job. Without explicit permissions, the GITHUB_TOKEN is granted default (potentially broad) permissions.

- `.github/workflows/package.yml`: No permissions block at top level or job level.
- `.github/workflows/release.yml`: No permissions block at top level or job level.

Locations:

- `.github/workflows/package.yml:1`
- `.github/workflows/release.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, github-env-injection, missing-permissions

**Notes:**

Fixed all four findings:
1. Pinned all unpinned `uses:` references to full 40-char SHAs: actions/checkout@v4→11d5960a..., codecov/codecov-action@v5.4.2→ad3126e9..., aws-actions/configure-aws-credentials@v4→7474bc46...
2. Fixed script-injection in .github/actions/build/action.yml by moving `${{ github.repository_owner }}` into the step's `env:` block as REPO_OWNER and referencing it as $REPO_OWNER in the shell script.
3. The github-env-injection finding is resolved by the same script-injection fix — the expression is no longer interpolated in the run block; the ARN values written to GITHUB_OUTPUT are static literals.
4. Added `permissions: contents: write` to package.yml and release.yml (both need to push to the repo). tests.yml already had a permissions block.

