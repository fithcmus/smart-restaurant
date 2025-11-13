# User Stories - Detailed Breakdown

**Version:** 1.0  
**Last Updated:** 2025-11-01  
**Owner:** Product Team

## Overview

This document contains a detailed breakdown of the MVP user stories into smaller, sprint-ready stories. Each story is sized to be completed within 1-3 days by a single developer or pair.

## Story Estimation Guide

- **XS (1 point):** < 1 day - Simple UI changes, configuration updates
- **S (2 points):** 1-2 days - Single feature with backend + frontend
- **M (3 points):** 2-3 days - Feature with multiple components or integration
- **L (5 points):** 3-5 days - Complex feature (should be avoided, consider splitting)

## Priority Legend

- **P0 (Critical):** Must have for MVP launch
- **P1 (High):** Important for good user experience
- **P2 (Medium):** Nice to have, can be post-MVP
- **P3 (Low):** Future enhancement

---

## Epic 1: Tenant Management

### FR-1-001: Basic Tenant Signup
**Priority:** P0 | **Story Points:** 2 (S) | **Sprint:** 1

**As a** restaurant owner  
**I want to** create an account with email and password  
**So that** I can access the platform

**Acceptance Criteria:**
- Given I'm on the signup page
- When I enter valid email, password, and restaurant name
- Then my account is created and I receive a verification email

**Technical Notes:**
- POST /api/auth/signup
- PostgreSQL tenant + user record
- Send email via SendGrid/SES

---

### FR-1-002: Email Verification
**Priority:** P0 | **Story Points:** 2 (S) | **Sprint:** 1

**As a** new tenant  
**I want to** verify my email address  
**So that** I can activate my account

**Acceptance Criteria:**
- Given I received a verification email
- When I click the verification link
- Then my email is marked as verified and I can log in

**Technical Notes:**
- JWT token in email link (24h expiry)
- Update user.emailVerified flag

---

### FR-1-003: Tenant Onboarding Wizard
**Priority:** P1 | **Story Points:** 3 (M) | **Sprint:** 2

**As a** verified tenant  
**I want to** complete a setup wizard  
**So that** I can configure my restaurant profile

**Acceptance Criteria:**
- Given I've verified my email
- When I first log in
- Then I see a 4-step wizard: Business Info → Operating Hours → Payment Setup → Review

**Technical Notes:**
- Multi-step form with progress indicator
- Save draft state to allow resuming
- Skip payment for now (P2)

---

### FR-1-004: Tenant Profile Management
**Priority:** P1 | **Story Points:** 2 (S) | **Sprint:** 2

**As a** tenant admin  
**I want to** edit my restaurant details  
**So that** I can keep information up to date

**Acceptance Criteria:**
- Given I'm logged in as tenant admin
- When I navigate to Settings → Business Profile
- Then I can edit name, logo, address, phone, hours

**Technical Notes:**
- PATCH /api/tenants/:id
- Logo upload to S3

---

## Epic 2: Menu Management

### FR-2-001: Create Menu Categories
**Priority:** P0 | **Story Points:** 2 (S) | **Sprint:** 1

**As a** restaurant manager  
**I want to** create menu categories  
**So that** I can organize my menu items

**Acceptance Criteria:**
- Given I'm on the Menu Management page
- When I click "Add Category" and enter name, description, display order
- Then the category appears in my menu structure

**Technical Notes:**
- POST /api/menu/categories
- Support drag-and-drop reordering

---

### FR-2-002: Create Menu Items
**Priority:** P0 | **Story Points:** 3 (M) | **Sprint:** 1

**As a** restaurant manager  
**I want to** add menu items to categories  
**So that** customers can see what we offer

**Acceptance Criteria:**
- Given I have at least one category
- When I add an item with name, price, description, image
- Then the item appears in the selected category

**Technical Notes:**
- POST /api/menu/items
- Image upload to S3 (optimize/resize)
- Price stored as integer cents

---

### FR-2-003: Update/Delete Menu Items
**Priority:** P0 | **Story Points:** 2 (S) | **Sprint:** 2

**As a** restaurant manager  
**I want to** edit or remove menu items  
**So that** I can keep my menu accurate

**Acceptance Criteria:**
- Given I have existing menu items
- When I click Edit/Delete
- Then changes are saved and reflected immediately

**Technical Notes:**
- PATCH /api/menu/items/:id
- DELETE /api/menu/items/:id (soft delete)
- Invalidate Redis cache on update

---

### FR-2-004: Menu Item Modifiers
**Priority:** P1 | **Story Points:** 3 (M) | **Sprint:** 3

**As a** restaurant manager  
**I want to** add modifiers to menu items  
**So that** customers can customize their orders

**Acceptance Criteria:**
- Given I'm editing a menu item
- When I add a modifier group (e.g., "Size" with options S/M/L)
- Then customers can select options when ordering

**Technical Notes:**
- Modifier groups: single-select or multi-select
- Price adjustments per option
- Required vs optional groups

---

### FR-2-005: Menu Availability Toggle
**Priority:** P1 | **Story Points:** 1 (XS) | **Sprint:** 2

**As a** restaurant manager  
**I want to** mark items as available/unavailable  
**So that** customers can't order out-of-stock items

**Acceptance Criteria:**
- Given I have menu items
- When I toggle availability
- Then the item is greyed out on customer menu

**Technical Notes:**
- Boolean flag on menu_items table
- Real-time update via WebSocket

---

## Epic 3: QR Code & Table Management

### FR-3-001: Add Tables
**Priority:** P0 | **Story Points:** 2 (S) | **Sprint:** 2

**As a** restaurant manager  
**I want to** add tables with identifiers  
**So that** I can manage my dining area

**Acceptance Criteria:**
- Given I'm on the Tables page
- When I add a table with number/name and section
- Then the table is saved to my restaurant

**Technical Notes:**
- POST /api/tables
- Support bulk import (CSV)

---

### FR-3-002: Generate QR Codes for Tables
**Priority:** P0 | **Story Points:** 3 (M) | **Sprint:** 2

**As a** restaurant manager  
**I want to** generate QR codes for each table  
**So that** customers can scan to order

**Acceptance Criteria:**
- Given I have tables
- When I click "Generate QR"
- Then a unique, signed QR code is created with embedded token

**Technical Notes:**
- Token format: `{tenantId}.{tableId}.{timestamp}.{hmacSignature}`
- QR contains URL: `https://app.unifiedordering.com/order?t={token}`
- Store QR image in S3

---

### FR-3-003: Download/Print QR Codes
**Priority:** P0 | **Story Points:** 2 (S) | **Sprint:** 3

**As a** restaurant manager  
**I want to** download QR codes as images or PDF  
**So that** I can print and place them on tables

**Acceptance Criteria:**
- Given I have generated QR codes
- When I click "Download All"
- Then I get a PDF with one QR per page labeled with table number

**Technical Notes:**
- Use pdfkit or puppeteer
- Include table number and instructions

---

### FR-3-004: Regenerate QR Codes
**Priority:** P2 | **Story Points:** 2 (S) | **Sprint:** 4

**As a** restaurant manager  
**I want to** regenerate QR codes  
**So that** I can invalidate old codes if compromised

**Acceptance Criteria:**
- Given I have existing QR codes
- When I click "Regenerate"
- Then old tokens are invalidated and new QR codes are created

**Technical Notes:**
- Increment version in token
- Old tokens return 410 Gone

---

## Epic 4: Customer Ordering Flow

### FR-4-001: Scan QR and Load Menu
**Priority:** P0 | **Story Points:** 3 (M) | **Sprint:** 3

**As a** customer  
**I want to** scan a QR code on my table  
**So that** I can view the restaurant's menu

**Acceptance Criteria:**
- Given I scan a QR code with my phone camera
- When the URL opens in my browser
- Then I see the menu with categories and items

**Technical Notes:**
- Verify HMAC signature
- Extract tenantId and tableId
- Fetch menu from cache (Redis) or DB

---

### FR-4-002: Add Items to Cart
**Priority:** P0 | **Story Points:** 2 (S) | **Sprint:** 3

**As a** customer  
**I want to** add menu items to my cart  
**So that** I can prepare my order

**Acceptance Criteria:**
- Given I'm viewing the menu
- When I click "+ Add" on an item
- Then it's added to my cart with quantity 1

**Technical Notes:**
- LocalStorage or session for cart state
- Show cart badge with item count

---

### FR-4-003: Customize Items with Modifiers
**Priority:** P1 | **Story Points:** 3 (M) | **Sprint:** 4

**As a** customer  
**I want to** select modifiers for items  
**So that** I can customize my order

**Acceptance Criteria:**
- Given I click on an item with modifiers
- When I select options and click "Add to Cart"
- Then the item is added with my selections and price adjustments

**Technical Notes:**
- Modal with modifier selection
- Validate required groups

---

### FR-4-004: Review Cart and Checkout
**Priority:** P0 | **Story Points:** 2 (S) | **Sprint:** 4

**As a** customer  
**I want to** review my cart and proceed to checkout  
**So that** I can confirm my order

**Acceptance Criteria:**
- Given I have items in my cart
- When I click "Checkout"
- Then I see order summary with subtotal, tax, total

**Technical Notes:**
- Tax calculation (configurable rate)
- Show table number

---

### FR-4-005: Enter Customer Name and Notes
**Priority:** P1 | **Story Points:** 1 (XS) | **Sprint:** 4

**As a** customer  
**I want to** enter my name and special instructions  
**So that** the restaurant knows who ordered

**Acceptance Criteria:**
- Given I'm at checkout
- When I enter optional name and notes
- Then they're included with my order

**Technical Notes:**
- Optional fields
- Max 500 chars for notes

---

### FR-4-006: Submit Order and Show Confirmation
**Priority:** P0 | **Story Points:** 2 (S) | **Sprint:** 5

**As a** customer  
**I want to** submit my order  
**So that** the kitchen receives it

**Acceptance Criteria:**
- Given I've completed checkout
- When I click "Place Order"
- Then I see a confirmation page with order number

**Technical Notes:**
- POST /api/orders
- Generate order number (e.g., #1234)
- Send WebSocket event to KDS

---

## Epic 5: Payment Integration

### FR-5-001: Stripe Payment Setup (Backend)
**Priority:** P0 | **Story Points:** 3 (M) | **Sprint:** 5

**As a** system  
**I want to** integrate Stripe payment API  
**So that** customers can pay by card

**Acceptance Criteria:**
- Given a customer submits an order
- When they provide card details
- Then payment is processed via Stripe

**Technical Notes:**
- POST /api/payments/create-intent
- Use Stripe Payment Intents API
- Store payment status in orders table

---

### FR-5-002: Card Payment UI
**Priority:** P0 | **Story Points:** 3 (M) | **Sprint:** 5

**As a** customer  
**I want to** enter my card details securely  
**So that** I can pay for my order

**Acceptance Criteria:**
- Given I'm at the payment step
- When I enter card number, expiry, CVV
- Then payment is processed via Stripe Elements

**Technical Notes:**
- Use Stripe Elements for PCI compliance
- Show loading state during payment
- Handle errors (insufficient funds, declined card)

---

### FR-5-003: Payment Confirmation
**Priority:** P0 | **Story Points:** 1 (XS) | **Sprint:** 5

**As a** customer  
**I want to** see payment confirmation  
**So that** I know my payment succeeded

**Acceptance Criteria:**
- Given my payment succeeded
- When processing completes
- Then I see "Payment Successful" with order details

**Technical Notes:**
- Update order status to 'paid'
- Show estimated prep time

---

### FR-5-004: Payment Failure Handling
**Priority:** P1 | **Story Points:** 2 (S) | **Sprint:** 6

**As a** customer  
**I want to** retry payment if it fails  
**So that** I can complete my order

**Acceptance Criteria:**
- Given my payment failed
- When I see the error message
- Then I can retry with same or different card

**Technical Notes:**
- Show specific error (card declined, network error)
- Allow retry without re-creating order

---

## Epic 6: Kitchen Display System (KDS)

### FR-6-001: View New Orders
**Priority:** P0 | **Story Points:** 2 (S) | **Sprint:** 6

**As a** kitchen staff  
**I want to** see new orders in real-time  
**So that** I can start preparing them

**Acceptance Criteria:**
- Given I'm on the KDS page
- When a new order arrives
- Then it appears at the top with sound/visual alert

**Technical Notes:**
- WebSocket subscription to tenant orders
- Show order time, table, items

---

### FR-6-002: Update Order Status
**Priority:** P0 | **Story Points:** 2 (S) | **Sprint:** 6

**As a** kitchen staff  
**I want to** mark orders as preparing/ready/delivered  
**So that** we can track order progress

**Acceptance Criteria:**
- Given I have a new order
- When I click "Start Preparing"
- Then status changes to 'preparing' and timer starts

**Technical Notes:**
- PATCH /api/orders/:id/status
- State machine: pending → preparing → ready → delivered

---

### FR-6-003: Real-time Order Updates
**Priority:** P1 | **Story Points:** 3 (M) | **Sprint:** 6

**As a** customer  
**I want to** see my order status update in real-time  
**So that** I know when it's ready

**Acceptance Criteria:**
- Given I placed an order
- When kitchen updates status
- Then my confirmation page updates automatically

**Technical Notes:**
- WebSocket channel per order
- Show status badge and progress bar

---

### FR-6-004: Order Timer and Alerts
**Priority:** P2 | **Story Points:** 2 (S) | **Sprint:** 7

**As a** kitchen manager  
**I want to** see how long each order has been in queue  
**So that** I can prioritize old orders

**Acceptance Criteria:**
- Given orders are in KDS
- When an order exceeds target time (e.g., 15 min)
- Then it's highlighted in red

**Technical Notes:**
- Calculate elapsed time client-side
- Configurable threshold per tenant

---

## Epic 7: Staff & Role Management

### FR-7-001: Invite Staff Members
**Priority:** P1 | **Story Points:** 2 (S) | **Sprint:** 7

**As a** tenant admin  
**I want to** invite staff via email  
**So that** they can access the system

**Acceptance Criteria:**
- Given I'm on the Staff page
- When I enter email and role
- Then an invitation email is sent

**Technical Notes:**
- POST /api/staff/invite
- Generate invitation token (7 days expiry)

---

### FR-7-002: Staff Signup via Invite
**Priority:** P1 | **Story Points:** 2 (S) | **Sprint:** 7

**As an** invited staff member  
**I want to** accept the invitation and create my account  
**So that** I can start working

**Acceptance Criteria:**
- Given I received an invitation email
- When I click the link and set my password
- Then my account is created with the assigned role

**Technical Notes:**
- Verify token and create user
- Link to tenant via tenantId

---

### FR-7-003: Role-Based Access Control (RBAC)
**Priority:** P1 | **Story Points:** 3 (M) | **Sprint:** 7

**As a** tenant admin  
**I want to** restrict access based on roles  
**So that** staff only see relevant features

**Acceptance Criteria:**
- Given different staff roles (admin, manager, kitchen, server)
- When a user logs in
- Then they see only permitted pages/actions

**Technical Notes:**
- Roles: admin (full), manager (menu, tables), kitchen (KDS only), server (orders only)
- Implement RBAC middleware
- PostgreSQL RLS policies

---

### FR-7-004: Manage Staff Roles
**Priority:** P2 | **Story Points:** 1 (XS) | **Sprint:** 8

**As a** tenant admin  
**I want to** change staff roles  
**So that** I can update permissions

**Acceptance Criteria:**
- Given I have staff members
- When I change a role in dropdown
- Then permissions update immediately

**Technical Notes:**
- PATCH /api/staff/:id/role

---

## Epic 8: Analytics Dashboard

### FR-8-001: Total Revenue Metric
**Priority:** P2 | **Story Points:** 2 (S) | **Sprint:** 8

**As a** restaurant manager  
**I want to** see total revenue  
**So that** I can track sales

**Acceptance Criteria:**
- Given I have completed orders
- When I view the Analytics page
- Then I see total revenue for selected period

**Technical Notes:**
- Aggregate SUM(total) from orders
- Filter by date range

---

### FR-8-002: Order Count and Average Order Value
**Priority:** P2 | **Story Points:** 2 (S) | **Sprint:** 8

**As a** restaurant manager  
**I want to** see order count and AOV  
**So that** I can understand customer behavior

**Acceptance Criteria:**
- Given I have orders
- When I view analytics
- Then I see total orders and average order value

**Technical Notes:**
- AOV = total revenue / order count

---

### FR-8-003: Popular Items Report
**Priority:** P2 | **Story Points:** 2 (S) | **Sprint:** 8

**As a** restaurant manager  
**I want to** see most-ordered items  
**So that** I can optimize my menu

**Acceptance Criteria:**
- Given I have order history
- When I view analytics
- Then I see top 10 items by quantity ordered

**Technical Notes:**
- Join orders → order_items → menu_items
- Group by item and count

---

### FR-8-004: Peak Hours Chart
**Priority:** P2 | **Story Points:** 3 (M) | **Sprint:** 9

**As a** restaurant manager  
**I want to** see orders by hour of day  
**So that** I can optimize staffing

**Acceptance Criteria:**
- Given I have order history
- When I view analytics
- Then I see a bar chart of orders per hour

**Technical Notes:**
- Extract hour from createdAt
- Use Chart.js or Recharts

---

## Epic 9: Order Tracking (Customer)

### FR-9-001: Track Order Status
**Priority:** P1 | **Story Points:** 2 (S) | **Sprint:** 6

**As a** customer  
**I want to** track my order status  
**So that** I know when it will be ready

**Acceptance Criteria:**
- Given I placed an order
- When I'm on the confirmation page
- Then I see live status updates (pending → preparing → ready)

**Technical Notes:**
- WebSocket subscription to order updates
- Show estimated time remaining

---

## Epic 10: Admin Authentication

### FR-10-001: Admin Login
**Priority:** P0 | **Story Points:** 2 (S) | **Sprint:** 1

**As a** tenant admin  
**I want to** log in with email and password  
**So that** I can access my dashboard

**Acceptance Criteria:**
- Given I have a verified account
- When I enter correct credentials
- Then I'm redirected to dashboard with JWT token

**Technical Notes:**
- POST /api/auth/login
- Return JWT with 7-day expiry
- Set httpOnly cookie

---

## Sprint Planning Overview

### Sprint 1 (P0 Foundation)
- FR-1-001: Basic Tenant Signup (2)
- FR-1-002: Email Verification (2)
- FR-2-001: Create Menu Categories (2)
- FR-2-002: Create Menu Items (3)
- FR-10-001: Admin Login (2)
- **Total:** 11 points

### Sprint 2 (P0 Menu & Tables)
- FR-1-003: Tenant Onboarding Wizard (3)
- FR-1-004: Tenant Profile Management (2)
- FR-2-003: Update/Delete Menu Items (2)
- FR-2-005: Menu Availability Toggle (1)
- FR-3-001: Add Tables (2)
- FR-3-002: Generate QR Codes (3)
- **Total:** 13 points

### Sprint 3 (P0 Ordering Flow - Part 1)
- FR-3-003: Download/Print QR Codes (2)
- FR-4-001: Scan QR and Load Menu (3)
- FR-4-002: Add Items to Cart (2)
- FR-2-004: Menu Item Modifiers (3)
- **Total:** 10 points

### Sprint 4 (P0 Ordering Flow - Part 2)
- FR-4-003: Customize Items with Modifiers (3)
- FR-4-004: Review Cart and Checkout (2)
- FR-4-005: Enter Customer Name (1)
- **Total:** 6 points

### Sprint 5 (P0 Payment & Order Submission)
- FR-4-006: Submit Order (2)
- FR-5-001: Stripe Backend (3)
- FR-5-002: Card Payment UI (3)
- FR-5-003: Payment Confirmation (1)
- **Total:** 9 points

### Sprint 6 (P0 KDS & P1 Tracking)
- FR-6-001: View New Orders (2)
- FR-6-002: Update Order Status (2)
- FR-6-003: Real-time Order Updates (3)
- FR-9-001: Track Order Status (2)
- FR-5-004: Payment Failure Handling (2)
- **Total:** 11 points

### Sprint 7 (P1 Staff Management & P2 Analytics Start)
- FR-7-001: Invite Staff (2)
- FR-7-002: Staff Signup (2)
- FR-7-003: RBAC (3)
- FR-6-004: Order Timer (2)
- **Total:** 9 points

### Sprint 8 (P2 Analytics & Remaining P2)
- FR-7-004: Manage Staff Roles (1)
- FR-8-001: Total Revenue (2)
- FR-8-002: Order Count & AOV (2)
- FR-8-003: Popular Items (2)
- FR-3-004: Regenerate QR Codes (2)
- **Total:** 9 points

### Sprint 9 (P2 Final Features)
- FR-8-004: Peak Hours Chart (3)
- Polish & bug fixes
- **Total:** 3-5 points

---

## Notes

- **Total Stories:** 39 (vs original 10)
- **Total Estimated Points:** 78
- **Average Story Size:** 2 points (ideal for agile)
- **MVP Completion:** End of Sprint 6 (all P0 features)
- **Sprints 7-9:** P1 and P2 features for better UX

## Definition of Done

Each story is considered complete when:
1. ✅ Code is written and passes all unit tests
2. ✅ Acceptance criteria verified via manual or automated tests
3. ✅ Code reviewed and approved
4. ✅ Merged to main branch
5. ✅ Deployed to staging environment
6. ✅ Product owner accepts the feature

## Multilingual & Localization Requirement

All user stories and features must support multilingual interfaces. The platform should allow users to select their preferred language (e.g., English, Vietnamese, etc.) at login or in profile/settings. All UI text, labels, and error messages must be translatable. User stories and wireframes should visually indicate language selection controls and consider text expansion for different languages.

- Add a language selector to login, signup, and profile management screens.
- Ensure menu, cart, and checkout screens can display content in multiple languages.
- Indicate in wireframes and user stories where translations or locale-specific content will appear.

