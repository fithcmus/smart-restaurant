# ADR 0001 — Authentication & Multi-tenant Strategy

Date: 2025-10-31
Status: Proposed

## Context
Unified Restaurant Ordering Platform is a multi-tenant SaaS platform where each tenant (restaurant) needs admin interfaces and customer-facing ordering pages. We must ensure tenant isolation, simple onboarding, and secure public QR-based links for customers.

## Decision
Use JWT access tokens for tenant admin APIs (short-lived) and HMAC-signed short tokens for public QR links. Keep a per-tenant secret for QR signing (or a global KMS-backed app key if per-tenant management is too heavy initially).

## Rationale
- JWTs are widely supported and easy to validate in stateless services.
- HMAC-signed QR tokens avoid exposing internal IDs and can be short-lived.
- Per-tenant secrets improve isolation; KMS simplifies rotation.

## Consequences
- Need key management (KMS) and rotation procedures.
- Public scan endpoint must validate HMAC tokens and map to tenant/table.
- Token revocation strategy: increment token version on table regenerate to invalidate previous tokens.

## Alternatives considered
- OAuth2 with refresh tokens: heavier for initial MVP.
- Database-backed session tokens: easier revocation but more stateful and scaling overhead.

