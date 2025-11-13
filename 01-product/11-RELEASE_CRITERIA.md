# Release Criteria

Version: 0.1
Date: 2025-10-31

This document defines the conditions that must be met before a release can be promoted to production.

## Required checks
- All acceptance criteria for scoped features are implemented and passing.
- CI: all unit and integration tests pass.
- E2E: critical user journeys pass in staging (smoke tests).
- Security: dependency scan results reviewed; no critical vulnerabilities.
- Performance: basic load test shows acceptable behavior for target traffic.
- Documentation: `docs/01-product/` updated with release notes and any user-facing changes.
- Rollback strategy & DB migrations reviewed and tested on staging.

## Release verification steps (post-deploy)
- Smoke test flows (ordering, admin changes) pass in production.
- Monitoring: key dashboards show healthy signals for 30 minutes.
- No critical errors or high error rates.

## Rollback conditions
- Critical user-facing defects not fixable by configuration in < 2 hours.
- Major payment or data-loss issues.

(Adapt criteria per release risk profile; e.g., major platform changes may require longer canary windows)
