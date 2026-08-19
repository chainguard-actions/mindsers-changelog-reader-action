<!-- markdownlint-disable -->

# Hardening Report: mindsers--changelog-reader-action/v2.4.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **mindsers--changelog-reader-action/v2.4.0** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow uses `mindsers/changelog-reader-action@v2`, which is pinned to a mutable tag rather than an immutable 40-character commit SHA. This means the action code can change without notice, enabling supply-chain attacks. It should be pinned to a full SHA digest (e.g., `mindsers/changelog-reader-action@<40-char-sha> # v2`).

Locations:

- `.github/workflows/release.yml:36`

### missing-permissions (severity: medium)

The workflow file `test.yml` has no top-level `permissions:` key, and neither the `unit` job nor the `e2e` job defines its own `permissions:` block. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad. A minimal `permissions:` block (e.g., `contents: read`) should be added at the top level or per job.

Locations:

- `.github/workflows/test.yml:1`

### github-env-injection (severity: high)

In the 'Resolve version and validate against package.json' step, the value `version` (derived from env var `TAG`, which holds `${{ github.ref_name }}`) is written directly to `$GITHUB_OUTPUT` via `echo "value=$version" >> "$GITHUB_OUTPUT"` without the required sanitization step (`printf '%s' "$version" | tr -d '\n\r'`). A tag name containing newline characters could inject arbitrary key-value pairs into the GitHub output environment, potentially overwriting subsequent step outputs.

Locations:

- `.github/workflows/release.yml:32`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, github-env-injection, missing-permissions

**Notes:**

Three fixes applied: (1) Pinned mindsers/changelog-reader-action@v2 to full SHA 1faaf50aa09d5793d9a100819973df801febfb31 in release.yml line 36. (2) Fixed github-env-injection in release.yml by sanitizing the version value with `safe=$(printf '%s' "$version" | tr -d '\n\r')` before writing `value=$safe` to GITHUB_OUTPUT. (3) Added `permissions: contents: read` top-level block to test.yml to address missing-permissions finding.

