<!-- markdownlint-disable -->

# Hardening Report: mindsers--changelog-reader-action/v2.2.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **mindsers--changelog-reader-action/v2.2.2** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow references `actions/checkout@v2` (a mutable tag) in two steps instead of a full 40-character commit SHA. A tag can be moved to point to a different, potentially malicious commit, enabling supply-chain attacks.

Locations:

- `.github/workflows/test.yml:14`
- `.github/workflows/test.yml:23`

### script-injection (severity: high)

Sub-rule (a): GitHub Actions expressions are interpolated directly inside `run:` shell command strings. The 'Display output' step interpolates `${{ steps.changelog_reader.outputs.version }}`, `${{ steps.changelog_reader.outputs.date }}`, `${{ steps.changelog_reader.outputs.status }}`, and `${{ steps.changelog_reader.outputs.changes }}` directly into shell commands. The 'Check output' step interpolates `${{ steps.changelog_reader.outputs.changes }}`. If the changelog content contains shell metacharacters, this allows command injection. These values should be passed via `env:` variables and referenced as quoted shell variables instead.

Locations:

- `.github/workflows/test.yml:29`
- `.github/workflows/test.yml:35`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and neither of its jobs (`unit`, `e2e`) defines a `permissions:` block. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (e.g., write access to contents). A minimal explicit `permissions:` block (e.g., `contents: read`) should be added.

Locations:

- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, missing-permissions

**Notes:**

Fixed all three findings in .github/workflows/test.yml: (1) Pinned both `actions/checkout@v2` references to full SHA `0717577d45739eb3c851188b29f50ed6c0b2194e` with `# v2` comment. (2) Moved all `${{ steps.changelog_reader.outputs.* }}` expressions out of `run:` shell strings into `env:` blocks in both the 'Display output' and 'Check output' steps, referencing them as plain shell variables. (3) Added top-level `permissions: contents: read` block to restrict the workflow to the minimum required permissions.

