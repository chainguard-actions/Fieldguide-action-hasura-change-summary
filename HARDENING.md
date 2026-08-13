<!-- markdownlint-disable -->

# Hardening Report: Fieldguide--action-hasura-change-summary/v2.4.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Fieldguide--action-hasura-change-summary/v2.4.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in workflow files are pinned to mutable tags rather than full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced tag is moved or the upstream action is compromised. Failing references: ci.yml — `actions/checkout@v3`, `actions/setup-node@v3`, `stefanzweifel/git-auto-commit-action@v4`; release.yml — `Actions-R-Us/actions-tagger@v2`. Each should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3`.

Locations:

- `.github/workflows/ci.yml:10`
- `.github/workflows/ci.yml:13`
- `.github/workflows/ci.yml:21`
- `.github/workflows/ci.yml:27`
- `.github/workflows/ci.yml:30`
- `.github/workflows/release.yml:8`

### missing-permissions (severity: medium)

Neither `.github/workflows/ci.yml` nor `.github/workflows/release.yml` declares a top-level `permissions:` block, and no individual job within either file declares its own `permissions:` block. Without explicit permissions, workflows run with the repository's default token permissions (which may be `write-all`), granting unnecessary access. Each workflow should declare the minimal required permissions (e.g. `permissions: contents: read`).

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/release.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references in both workflow files by resolving each tag to its full 40-character commit SHA (preserving the tag as a comment). Added top-level `permissions: contents: write` to both ci.yml and release.yml — this is the minimum required since ci.yml uses git-auto-commit-action (needs to push commits) and release.yml uses actions-tagger (needs to create/update tags).

