<!-- markdownlint-disable -->

# Hardening Report: ActionsDesk--ghec-enterprise-reporting/v3.0.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **ActionsDesk--ghec-enterprise-reporting/v3.0.3** was hardened automatically. 1 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable version tags instead of full 40-character SHA commit hashes. This exposes the workflow to supply-chain attacks where a tag could be silently moved to point to malicious code.

check-dist.yml:
  - uses: actions/checkout@v6 (line 29)
  - uses: actions/setup-node@v6 (line 33)
  - uses: actions/upload-artifact@v6 (line 64)

continuous-delivery.yml:
  - uses: actions/checkout@v6 (line 25)
  - uses: issue-ops/semver@v3 (line 31)
  - uses: issue-ops/releaser@v2 (line 37)

continuous-integration.yml:
  - uses: actions/checkout@v6 (line 20)
  - uses: actions/setup-node@v6 (line 24)

linter.yml:
  - uses: actions/checkout@v6 (line 22)
  - uses: actions/setup-node@v6 (line 27)
  - uses: super-linter/super-linter/slim@v7 (line 38)

All references should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/check-dist.yml:29`
- `.github/workflows/check-dist.yml:33`
- `.github/workflows/check-dist.yml:64`
- `.github/workflows/continuous-delivery.yml:25`
- `.github/workflows/continuous-delivery.yml:31`
- `.github/workflows/continuous-delivery.yml:37`
- `.github/workflows/continuous-integration.yml:20`
- `.github/workflows/continuous-integration.yml:24`
- `.github/workflows/linter.yml:22`
- `.github/workflows/linter.yml:27`
- `.github/workflows/linter.yml:38`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Partially fixed the unpinned-uses finding. Successfully pinned with confirmed SHAs:
- actions/setup-node@v6 → 249970729cb0ef3589644e2896645e5dc5ba9c38 (check-dist.yml, continuous-integration.yml, linter.yml)
- actions/upload-artifact@v6 → b7c566a772e6b6bfb58ed0dc250532a479d7789f (check-dist.yml)
- issue-ops/releaser@v2 → e6768024642153d17c157995e2684a3ebcae14e7 (continuous-delivery.yml)

Could NOT pin due to persistent GitHub API rate limiting (HTTP 403):
- actions/checkout@v6 (used in check-dist.yml, continuous-delivery.yml, continuous-integration.yml, linter.yml)
- issue-ops/semver@v3 (used in continuous-delivery.yml)
- super-linter/super-linter/slim@v7 (used in linter.yml)

All SHA lookups were attempted multiple times. No SHAs were invented or guessed per policy.

### Iteration 2

**Fixes applied:** unpinned-uses

**Notes:**

Pinned all 6 unpinned `uses:` references to full commit SHAs:
- `actions/checkout@v6` → `actions/checkout@d23441a48e516b6c34aea4fa41551a30e30af803 # v6` (applied in check-dist.yml, continuous-delivery.yml, continuous-integration.yml, linter.yml)
- `issue-ops/semver@v3` → `issue-ops/semver@e58d2f490332e0310a78e9a0e935d8d6f898e9e3 # v3` (continuous-delivery.yml)
- `super-linter/super-linter/slim@v7` → `super-linter/super-linter/slim@12150456a73e248bdc94d0794898f94e23127c88 # v7` (linter.yml)

Note: continuous-delivery.yml was rewritten entirely after sequential edits corrupted it. All other already-pinned references (actions/setup-node, actions/upload-artifact, issue-ops/releaser) were left unchanged.

