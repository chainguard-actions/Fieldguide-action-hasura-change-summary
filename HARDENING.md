<!-- markdownlint-disable -->

# Hardening Report: Fieldguide--action-hasura-change-summary/v4.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Fieldguide--action-hasura-change-summary/v4.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files use mutable tag references instead of pinned full-length commit SHAs, making the workflows vulnerable to supply-chain attacks if any of these actions are compromised or their tags are moved.

.github/workflows/ci.yml failing references:
- uses: actions/checkout@v6 (appears twice)
- uses: pnpm/action-setup@v4 (appears twice)
- uses: actions/setup-node@v6 (appears twice)
- uses: stefanzweifel/git-auto-commit-action@v7

.github/workflows/release.yml failing references:
- uses: Actions-R-Us/actions-tagger@v2

Locations:

- `.github/workflows/ci.yml:10`
- `.github/workflows/ci.yml:12`
- `.github/workflows/ci.yml:13`
- `.github/workflows/ci.yml:19`
- `.github/workflows/ci.yml:23`
- `.github/workflows/ci.yml:24`
- `.github/workflows/ci.yml:25`
- `.github/workflows/release.yml:8`

### missing-permissions (severity: medium)

Neither .github/workflows/ci.yml nor .github/workflows/release.yml defines a top-level `permissions:` key, and no individual job within either file defines a `permissions:` key. Without explicit permissions, workflows run with the default (potentially broad) token permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/release.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files:

.github/workflows/ci.yml:
- Pinned actions/checkout@v6 → @d23441a48e516b6c34aea4fa41551a30e30af803 # v6 (used twice)
- Pinned pnpm/action-setup@v4 → @b906affcce14559ad1aafd4ab0e942779e9f58b1 # v4 (used twice)
- Pinned actions/setup-node@v6 → @249970729cb0ef3589644e2896645e5dc5ba9c38 # v6 (used twice)
- Pinned stefanzweifel/git-auto-commit-action@v7 → @4a55954c782fc1ea30b9056cd3e7a2b40ca8887d # v7
- Added top-level `permissions: {}` (deny-all default)
- Added job-level `permissions: contents: write` for build job (needs to push commits)
- Added job-level `permissions: contents: read` for test job

.github/workflows/release.yml:
- Pinned Actions-R-Us/actions-tagger@v2 → @330ddfac760021349fef7ff62b372f2f691c20fb # v2
- Added top-level `permissions: {}` (deny-all default)
- Added job-level `permissions: contents: write` for git job (needs to create/update tags)

