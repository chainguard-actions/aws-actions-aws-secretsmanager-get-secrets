<!-- markdownlint-disable -->

# Hardening Report: aws-actions--aws-secretsmanager-get-secrets/v1.0.6

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **aws-actions--aws-secretsmanager-get-secrets/v1.0.6** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All three workflow files use action references pinned to mutable tags rather than full 40-character commit SHAs. Failing references: package.yml uses actions/checkout@v3; release.yml uses actions/checkout@v3; tests.yml uses actions/checkout@v3 and codecov/codecov-action@v3.1.4. These should be pinned to immutable commit SHAs (e.g. actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3).

Locations:

- `.github/workflows/package.yml:12`
- `.github/workflows/release.yml:9`
- `.github/workflows/tests.yml:13`
- `.github/workflows/tests.yml:17`

### missing-permissions (severity: medium)

None of the workflow files define a top-level permissions: key, and no job in any file defines a job-level permissions: key. This means workflows run with the default (potentially broad) GITHUB_TOKEN permissions. All three files are affected: package.yml, release.yml, and tests.yml.

Locations:

- `.github/workflows/package.yml:1`
- `.github/workflows/release.yml:1`
- `.github/workflows/tests.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all three workflow files: (1) Pinned all action references to full commit SHAs — actions/checkout@v3 → @f43a0e5ff2bd294095638e18286ca9a3d1956744 in package.yml, release.yml, and tests.yml; codecov/codecov-action@v3.1.4 → @eaaf4bedf32dbdc6b720b63067d99c4d77d6047d in tests.yml. (2) Added top-level `permissions: {}` to all three files to deny all permissions by default, with minimal job-level overrides: contents:write for package.yml and release.yml (both perform git push), and contents:read for tests.yml (checkout only).

