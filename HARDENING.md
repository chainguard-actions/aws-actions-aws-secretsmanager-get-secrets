<!-- markdownlint-disable -->

# Hardening Report: aws-actions--aws-secretsmanager-get-secrets/v3.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `374911c4be584df24e6da57af2d990570ef0f859`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **aws-actions--aws-secretsmanager-get-secrets/v3.0.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): a GitHub Actions expression is placed directly inside a run: script in the 'Determine role to assume' step: `if [ "${{ github.event_name }}" == "push" ]; then`. Fix: move the value into an env: variable or use the built-in $GITHUB_EVENT_NAME, and quote it: `if [ "$GITHUB_EVENT_NAME" == "push" ]`.

Locations:

- `.github/actions/build/action.yml:20`

### unpinned-uses (severity: high)

A uses: reference points to a mutable tag instead of a full 40-character commit SHA: `uses: aws-actions/configure-aws-credentials@v6`. Fix: pin it to a commit SHA, with the tag in a comment.

Locations:

- `.github/actions/build/action.yml:28`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

In .github/actions/build/action.yml, the 'Determine role to assume' step no longer puts ${{ github.event_name }} straight into the run: script. The value now goes into an env: variable (EVENT_NAME) and the script compares the quoted "$EVENT_NAME". aws-actions/configure-aws-credentials@v6 is now pinned to commit e1253824e5c10ff9df46874f81ed3ec929e19cfd, with '# v6' kept as a comment. I looked up that SHA from the v6 tag rather than guessing it. Nothing else was changed, and I haven't run anything to test it.

