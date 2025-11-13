# User Stories

Version: 0.2
Date: 2025-10-31

Priority: High -> Low (top is highest priority for MVP)

Each story includes acceptance criteria (Gherkin-style where useful) so QA can convert them to tests.

---

## FR-1: Tenant Signup & Onboarding
- ID: FR-1-001
- Title: Tenant signs up and completes basic onboarding
- As a: Restaurant owner
- I want: to sign up, create my restaurant profile, and set opening hours
- So that: I can start generating QR codes and manage my menu
- Acceptance Criteria:
  - Given a valid email and password, when I sign up, then a tenant account is created and I receive a confirmation email
  - Given I navigate to onboarding, when I enter restaurant name, timezone, and hours, then the tenant profile is saved and marked complete

---

## FR-2: Menu & Catalog Management
- ID: FR-2-001
- Title: Admin can create categories and menu items
- As a: Tenant admin
- I want: to add/edit/delete categories, items, and modifiers (e.g., size, extras)
- So that: I can maintain the menu available to customers
- Acceptance Criteria:
  - When I add an item with price and category, the item appears in the tenant menu
  - When I add modifiers, they are presented as options in the item detail

- ID: FR-2-002
- Title: Admin can publish/unpublish menu changes
- As a: Tenant admin
- I want: to control whether changes are live
- So that: I can prepare changes and publish when ready
- Acceptance Criteria:
  - Draft changes do not appear to customers until published

---

## FR-3: Table Management & QR Generation
- ID: FR-3-001
- Title: Admin can create tables and generate QR codes
- As a: Tenant admin
- I want: to create table records (name, capacity, location) and generate printable QR PNG/SVG for each table
- So that: customers can scan the QR and order tied to a table
- Acceptance Criteria:
  - Given a table is created, an endpoint returns a QR image and a public URL containing a signed token
  - When an admin regenerates the QR, the previous token becomes invalid

---

## FR-4: Customer Ordering (Core Flow)
- ID: FR-4-001
- Title: Customer scans table QR and places an order
- As a: Customer
- I want: scan QR → view menu → add items/customizations → checkout
- So that: I can order from my table without staff assistance
- Acceptance Criteria:
  - Given a valid QR, when customer opens the link, they see the tenant menu for that table
  - When customer adds items and proceeds to checkout, an order is created with status `Received`
  - When order is created, staff are notified and the order appears in the tenant's order list/KDS

- ID: FR-4-002
- Title: Customer can edit cart before checkout
- Acceptance Criteria:
  - Customer may add/remove items, change quantities, and update modifiers before confirming

- ID: FR-4-003
- Title: Cart/session persists during short visits
- Acceptance Criteria:
  - If the customer reloads the page within 30 minutes, the cart is preserved for that table session

---

## FR-5: Payment & Checkout Options
- ID: FR-5-001
- Title: Customer can pay with card at checkout (guest flow)
- As a: Customer
- I want: to enter card details and complete payment
- So that: the order is paid and confirmed
- Acceptance Criteria:
  - Successful payment updates order status to `Paid` and receipts are generated
  - Failed payment returns a friendly error and allows retry

- ID: FR-5-002
- Title: Support table-bill / pay-later option
- Acceptance Criteria:
  - Customer may choose `Bill to table` and the order is created with status `Pending payment`

---

## FR-6: Order Processing & KDS
- ID: FR-6-001
- Title: Staff view new orders and update states
- As a: Kitchen/Staff user
- I want: to see incoming orders, accept them, and mark them ready
- So that: the kitchen can manage preparation and handoff
- Acceptance Criteria:
  - New orders appear in the orders list sorted by created time
  - Staff can change order status: `Received` → `Preparing` → `Ready` → `Completed`
  - Status changes are persisted and visible to customers/admins

- ID: FR-6-002
- Title: Order notifications
- Acceptance Criteria:
  - When new order arrives, staff UI shows a badge/notification and optional sound

---

## FR-7: Admin Controls & Permissions
- ID: FR-7-001
- Title: Tenant admins can invite staff users with roles
- As a: Tenant admin
- I want: to invite staff and assign roles (admin, staff, kitchen)
- So that: staff can manage orders and menu according to permissions
- Acceptance Criteria:
  - Invited users receive email and can accept invitation to join tenant
  - Role-based access controls prevent unauthorized actions

---

## FR-8: Analytics & Reporting
- ID: FR-8-001
- Title: Tenant admin can view basic analytics
- As a: Tenant admin
- I want: to see orders/day, conversion rate, and AOV
- So that: I can measure performance and optimize menu/prices
- Acceptance Criteria:
  - Dashboard shows orders count, conversion (scans→orders), and AOV for selected date range

---

## FR-9: Error Handling & Support Flows
- ID: FR-9-001
- Title: Friendly errors for expired/invalid QR
- As a: Customer
- I want: to see a clear message when QR is invalid/expired and contact info
- So that: I can get help without confusion
- Acceptance Criteria:
  - Public scan endpoint returns a user-friendly page explaining the issue and next steps

---

## FR-10: Admin Maintenance
- ID: FR-10-001
- Title: Admin can deactivate a table
- Acceptance Criteria:
  - Once deactivated, scanning the table QR shows an inactive message and prevents ordering

---

## Notes & Next steps
- These stories are intentionally concise; convert high-value ones to more granular tasks (backend endpoints, frontend screens, tests)
- Link each story to SRS IDs (e.g., FR-4 in SRS) when creating tickets

(End of user stories for MVP — add lower priority stories in the backlog)

