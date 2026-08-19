<!-- markdownlint-disable -->

# Hardening Report: mindsers--changelog-reader-action/v2.2.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **mindsers--changelog-reader-action/v2.2.3** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow file uses actions/checkout@v2 (a mutable tag) in two steps instead of a full 40-character commit SHA. If the tag is moved or the repository is compromised, the action could execute arbitrary code. All uses: references should be pinned to a full SHA, e.g. actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v2.

Locations:

- `.github/workflows/test.yml:14`
- `.github/workflows/test.yml:21`

### permissions (severity: medium)

The workflow file has no top-level permissions: key and neither the 'unit' nor the 'e2e' job defines its own permissions: block. Without explicit permissions, the workflow inherits the default repository permissions (which may include write access to contents, pull-requests, etc.), violating the principle of least privilege. A minimal permissions block (e.g. permissions: contents: read) should be added.

Locations:

- `.github/workflows/test.yml:1`

### script-injection (severity: high)

The 'Display output' and 'Check output' steps in the e2e job interpolate ${{ steps.changelog_reader.outputs.version }}, ${{ steps.changelog_reader.outputs.date }}, ${{ steps.changelog_reader.outputs.status }}, and ${{ steps.changelog_reader.outputs.changes }} directly inside run: shell commands (sub-rule a). These expressions are substituted by the Actions template engine before the shell sees them, so a malicious changelog entry could inject arbitrary shell commands. The values should be passed via env: variables and then referenced as quoted shell variables (e.g. "$CHANGES") instead.

Locations:

- `.github/workflows/test.yml:27`
- `.github/workflows/test.yml:28`
- `.github/workflows/test.yml:29`
- `.github/workflows/test.yml:30`
- `.github/workflows/test.yml:33`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, permissions, script-injection

**Notes:**

Fixed all three findings in hardened/action/.github/workflows/test.yml: (1) Pinned both actions/checkout@v2 references to full SHA ee0669bd1cc54295c223e0bb666b733df41de1c5 # v2; (2) Added top-level 'permissions: contents: read' block; (3) Moved all ${{ steps.changelog_reader.outputs.* }} template expressions in the 'Display output' and 'Check output' steps into env: blocks (VERSION, DATE, STATUS, CHANGES) and replaced inline interpolations with quoted shell variable references ($VERSION, $DATE, $STATUS, $CHANGES).

