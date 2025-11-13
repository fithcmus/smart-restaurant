# Metrics & KPIs

Version: 0.1
Date: 2025-10-31

This document defines the core metrics we will track to validate product success. For each metric include: definition, owner, collection method, dashboard location, and target.

## Core metrics

- Activation / Onboarding Completion
  - Definition: % of tenants who complete onboarding within 7 days of signup
  - Owner: Product
  - Collection: backend event `tenant.onboarding.completed`
  - Dashboard: Tenant Dashboard > Onboarding
  - Target: 70% within 7 days

- Customer Conversion (table QR flow)
  - Definition: % of QR scans that result in a completed order
  - Owner: Product / Growth
  - Collection: events `qr.scan`, `order.created`
  - Target: 10% (initial)

- Average Order Value (AOV)
  - Definition: average order total for completed orders
  - Owner: Finance
  - Collection: orders table
  - Target: $X

- Time to Serve (order lifecycle)
  - Definition: median time from order created → order ready
  - Owner: Operations
  - Collection: order state transitions
  - Target: < 20 minutes

- Errors / Failures (SRE)
  - Definition: 5xx errors per 1k requests, failed payments per 100 orders
  - Owner: SRE
  - Collection: logs/monitoring
  - Target: < 1 per 1k requests

## Events & instrumentation guidance
- Prefer structured events with stable schema (see `docs/11-analytics/EVENT_SCHEMA.md`)
- Include correlation ids: requestId, tenantId, orderId

## Reporting cadence
- Daily: technical health and errors
- Weekly: activation and conversion trends
- Monthly: business KPIs (AOV, revenue)

(Add or refine metrics as you get telemetry data)
