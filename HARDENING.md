<!-- markdownlint-disable -->

# Hardening Report: Fieldguide--action-hasura-change-summary/v3.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Fieldguide--action-hasura-change-summary/v3.0.0** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references in ci.yml use mutable tags instead of pinned SHA digests, making the workflow vulnerable to supply-chain attacks if the referenced actions are compromised or their tags are moved. Failing references: `actions/checkout@v4` (lines 11, 26), `actions/setup-node@v4` (lines 14, 27), `stefanzweifel/git-auto-commit-action@v5` (line 20).

Locations:

- `.github/workflows/ci.yml:11`
- `.github/workflows/ci.yml:14`
- `.github/workflows/ci.yml:20`
- `.github/workflows/ci.yml:26`
- `.github/workflows/ci.yml:27`

### unpinned-uses (severity: high)

The `uses:` reference in release.yml uses a mutable tag instead of a pinned SHA digest. Failing reference: `Actions-R-Us/actions-tagger@v2` (line 9).

Locations:

- `.github/workflows/release.yml:9`

### missing-permissions (severity: medium)

ci.yml has no top-level `permissions:` key and neither of its jobs (`build`, `test`) defines a job-level `permissions:` block. Without explicit permissions, the workflow runs with the default (potentially broad) token permissions.

Locations:

- `.github/workflows/ci.yml:1`

### missing-permissions (severity: medium)

release.yml has no top-level `permissions:` key and its only job (`git`) defines no job-level `permissions:` block. Without explicit permissions, the workflow runs with the default (potentially broad) token permissions.

Locations:

- `.github/workflows/release.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all findings in both workflow files:

ci.yml:
- Pinned actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262 # v4 (lines 11, 26)
- Pinned actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020 # v4 (lines 14, 27)
- Pinned stefanzweifel/git-auto-commit-action@v5 → @b863ae1933cb653a53c021fe36dbb774e1fb9403 # v5 (line 20)
- Added top-level `permissions: {}` to deny all by default
- Added `permissions: contents: write` to the build job (needs to commit built artifacts)
- Added `permissions: contents: read` to the test job (only needs to read code)

release.yml:
- Pinned Actions-R-Us/actions-tagger@v2 → @330ddfac760021349fef7ff62b372f2f691c20fb # v2 (line 9)
- Added top-level `permissions: {}` to deny all by default
- Added `permissions: contents: write` to the git job (needs to create/update release tags)

