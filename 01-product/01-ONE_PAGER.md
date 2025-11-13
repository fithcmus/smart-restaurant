# One-pager / Problem Statement

Version: 0.1
Date: 2025-10-31
Author: 
Status: Draft

## Problem
Many small and medium restaurants lack a simple, affordable way to offer mobile ordering from the table. Customers wait for staff to take orders, causing slower service and lost incremental revenue. Tenants currently use fragmented tools (paper, generic POS) that don't provide a quick dine-in QR ordering flow.

## Target users
- Primary: Independent restaurant owners / managers who operate single or few-location restaurants and want a low-friction ordering solution.
- Secondary: Diners (customers) who want faster service and contactless ordering.

## Proposed solution
Provide a lightweight, multi-tenant platform that lets restaurants generate a QR per table which opens a tenant-branded mobile menu where customers can browse, customize items, and place orders. Orders route to staff via a simple KDS/order list and support payment or table-bill workflows.

## MVP features (concise)
- Tenant signup & basic onboarding (create restaurant, hours, location)
- QR generation per table (printable PNG/SVG + signed token)
- Menu management: categories, items, modifiers, prices
- Customer ordering: view menu, add to cart, checkout (card/guest), create order
- Staff order processing: order list, basic state transitions (Received → Preparing → Ready)
- Basic analytics: orders per day, conversion from QR scan to order

## Success metrics (initial targets)
- Onboarding activation: 70% of signed-up tenants complete onboarding in 7 days
- QR conversion: 10% of QR scans become completed orders (initial benchmark)
- Average order value (AOV): $X (tenant-defined target)
- Time-to-serve: median under 20 minutes for standard items

## Risks & mitigations
- Risk: Payment integration complexity — Mitigation: start with an offload model (redirect to payment provider) and add native integration later.
- Risk: Token leakage or misuse — Mitigation: signed short/long-lived QR tokens and ability to revoke/regenerate.
- Risk: Tenant data isolation — Mitigation: enforce tenant-scoped APIs and use per-tenant keys where feasible.

## Next steps
1. Convert SRS functional sections into prioritized user stories (docs/01-product/06-USER_STORIES.md).
2. Define minimal OpenAPI for core flows (tenants, menus, tables, orders) and scaffold backend.
3. Publish product docs site and link from README.

## Diagrams
- [User Journeys](./diagrams/user-journeys.md) — Customer, Admin, and Staff journey maps
- [System Architecture](./diagrams/system-architecture.md) — High-level architecture overview
- [Ordering Flow](./diagrams/ordering-flow.md) — End-to-end customer ordering sequence
- [QR Generation Flow](./diagrams/qr-generation-flow.md) — QR code generation and validation
- [Order State Machine](./diagrams/order-state-machine.md) — Order lifecycle and state transitions

> Created: 2025-10-31  
> Updated: 2025-11-01 (added diagram links)
