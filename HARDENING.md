<!-- markdownlint-disable -->

# Hardening Report: mindsers--changelog-reader-action/v2.2.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **mindsers--changelog-reader-action/v2.2.1** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow references `actions/checkout@v2` in two steps using a mutable version tag instead of a full 40-character commit SHA. This means a compromised or altered tag could silently substitute malicious code. Both occurrences must be pinned to a specific commit SHA (e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v2`).

Locations:

- `.github/workflows/test.yml:14`
- `.github/workflows/test.yml:23`

### script-injection (severity: high)

Sub-rule (a): GitHub Actions expressions (`${{ steps.changelog_reader.outputs.* }}`) are interpolated directly inside `run:` shell command strings. Even though `steps.*.outputs.*` may appear benign here, any expression interpolated directly into a shell script is a script-injection risk — the value is substituted into the shell command before the shell parses it, allowing shell metacharacters to be injected. Affected lines include the 'Display output' step (echoing version, date, status, changes) and the 'Check output' step. These values should be passed via `env:` variables and referenced as `"$VAR"` inside the script instead.

Locations:

- `.github/workflows/test.yml:29`
- `.github/workflows/test.yml:35`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and neither of its jobs (`unit`, `e2e`) defines a `permissions:` block. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (e.g. write access to contents). A minimal `permissions:` block (e.g. `contents: read`) should be added at the top level or to each job.

Locations:

- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, missing-permissions

**Notes:**

Fixed all three findings in .github/workflows/test.yml: (1) Pinned both `actions/checkout@v2` references to full SHA `ee0669bd1cc54295c223e0bb666b733df41de1c5 # v2`. (2) Moved all `${{ steps.changelog_reader.outputs.* }}` expressions out of `run:` shell strings into `env:` blocks (VERSION, DATE, STATUS, CHANGES), referencing them as plain `$VAR` in the shell. (3) Added a top-level `permissions: contents: read` block to enforce least-privilege token access.

