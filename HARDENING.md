<!-- markdownlint-disable -->

# Hardening Report: aws-actions--aws-secretsmanager-get-secrets/v3.0.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `374911c4be584df24e6da57af2d990570ef0f859`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **aws-actions--aws-secretsmanager-get-secrets/v3.0.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a): a GitHub Actions expression is put straight into a run: script in the internal composite action's 'Determine role to assume' step: `if [ "${{ github.event_name }}" == "push" ]; then`. Move the value into an env: variable and compare against the quoted variable instead, or use the built-in "$GITHUB_EVENT_NAME".

Locations:

- `.github/actions/build/action.yml:20`

### unpinned-uses (severity: high)

The internal composite action uses an action by tag, not by full commit SHA: `uses: aws-actions/configure-aws-credentials@v6`. Pin it to a 40-character commit SHA.

Locations:

- `.github/actions/build/action.yml:29`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

Both findings are fixed in .github/actions/build/action.yml. I haven't run anything to test the changes.

- script-injection: in the 'Determine role to assume' step, `${{ github.event_name }}` now goes into the step's env: block as EVENT_NAME, and the script compares the quoted "$EVENT_NAME" instead.
- unpinned-uses: aws-actions/configure-aws-credentials is now pinned to the full commit SHA e1253824e5c10ff9df46874f81ed3ec929e19cfd (looked up from the v6 tag), with a `# v6` comment kept for readability.

