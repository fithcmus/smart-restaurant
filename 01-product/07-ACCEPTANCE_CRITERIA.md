# Acceptance Criteria

Version: 0.2
Date: 2025-11-01
Author: 
Status: QA-Ready

This document contains formal acceptance tests for critical flows using Gherkin syntax (Given-When-Then). Use these as the basis for automated and manual QA test cases.

Related: [06-USER_STORIES.md](./06-USER_STORIES.md), [02-SRS.md](./02-SRS.md)

---

## FR-1: Tenant Signup & Onboarding

### AC-1.1: Successful tenant signup
**Story:** FR-1-001

```gherkin
Feature: Tenant Signup
  As a restaurant owner
  I want to sign up for an account
  So that I can start using the platform

Scenario: Successful signup with valid credentials
  Given I am on the signup page
  When I enter a valid email "owner@restaurant.com"
  And I enter a password "SecurePass123!"
  And I accept the terms of service
  And I click "Sign Up"
  Then my account is created
  And I receive a confirmation email at "owner@restaurant.com"
  And I am redirected to the onboarding wizard

Scenario: Signup with existing email
  Given a tenant account already exists with email "existing@restaurant.com"
  When I try to sign up with email "existing@restaurant.com"
  Then I see an error message "Email already registered"
  And I remain on the signup page

Scenario: Signup with weak password
  Given I am on the signup page
  When I enter a password "123"
  Then I see a validation error "Password must be at least 8 characters"
  And the signup button is disabled
```

### AC-1.2: Complete onboarding
**Story:** FR-1-001

```gherkin
Feature: Tenant Onboarding
  As a restaurant owner
  I want to complete my restaurant profile
  So that I can configure my restaurant settings

Scenario: Complete onboarding with required fields
  Given I have signed up and received a confirmation email
  When I navigate to the onboarding wizard
  And I enter restaurant name "Bella Italia"
  And I select timezone "America/New_York"
  And I set operating hours "Mon-Sun 11:00-22:00"
  And I click "Complete Onboarding"
  Then my tenant profile is saved
  And I see a success message "Welcome to Unified Restaurant Ordering Platform!"
  And I am redirected to the admin dashboard
  And my onboarding status is marked "complete"

Scenario: Skip optional onboarding steps
  Given I am on the onboarding wizard
  When I skip uploading a logo
  And I skip entering a description
  And I complete required fields only
  Then my profile is saved successfully
  And I can add optional details later
```

---

## FR-2: Menu & Catalog Management

### AC-2.1: Create and manage menu items
**Story:** FR-2-001

```gherkin
Feature: Menu Item Management
  As a tenant admin
  I want to create and edit menu items
  So that customers can see my offerings

Scenario: Create a new menu item
  Given I am logged in as a tenant admin
  And I navigate to the menu management page
  When I click "Add Item"
  And I enter item name "Margherita Pizza"
  And I select category "Main Dishes"
  And I enter price "12.99"
  And I upload an image "pizza.jpg"
  And I click "Save"
  Then the item "Margherita Pizza" appears in the menu list
  And the item is visible to customers in that tenant's menu

Scenario: Add modifiers to a menu item
  Given I have created a menu item "Coffee"
  When I navigate to edit "Coffee"
  And I add a modifier group "Size" with options ["Small ($2)", "Medium ($3)", "Large ($4)"]
  And I add a modifier group "Extras" with options ["Extra Shot ($0.50)", "Oat Milk ($0.75)"]
  And I save the item
  Then customers see the modifier options when viewing "Coffee"

Scenario: Delete a menu item
  Given I have a menu item "Old Dish"
  When I select "Old Dish" and click "Delete"
  And I confirm the deletion
  Then "Old Dish" is removed from the menu
  And customers no longer see "Old Dish"
```

### AC-2.2: Publish and unpublish menu changes
**Story:** FR-2-002

```gherkin
Feature: Menu Publishing
  As a tenant admin
  I want to control when menu changes go live
  So that I can prepare changes without affecting customers

Scenario: Draft changes are not visible to customers
  Given I have made changes to item "Burger" (new price: $15)
  And the changes are in "Draft" status
  When a customer views the menu
  Then they see the old price "$12" for "Burger"
  And they do not see the draft changes

Scenario: Publish menu changes
  Given I have draft changes (3 items edited, 2 items added)
  When I click "Publish Changes"
  And I confirm the publish action
  Then all draft changes go live immediately
  And customers see the updated menu
  And I see a success message "Menu published successfully"
```

---

## FR-3: Table Management & QR Generation

### AC-3.1: Create tables and generate QR codes
**Story:** FR-3-001

```gherkin
Feature: Table and QR Code Management
  As a tenant admin
  I want to create tables and generate QR codes
  So that customers can order from their table

Scenario: Create a new table
  Given I am logged in as a tenant admin
  When I navigate to "Tables"
  And I click "Add Table"
  And I enter table name "Table 5"
  And I enter capacity "4"
  And I enter location "Patio"
  And I click "Create"
  Then the table "Table 5" is created
  And a QR code is automatically generated
  And I see the QR code image and download link

Scenario: Download printable QR code
  Given I have created table "Table 5"
  When I click "Download QR"
  Then I receive a high-resolution PNG file (300 DPI)
  And the file is named "table-5-qr.png"
  And the QR encodes a signed token with table and tenant info

Scenario: Regenerate QR code
  Given table "Table 5" has an existing QR code with token version 1
  When I click "Regenerate QR"
  And I confirm the action
  Then a new QR code is generated with token version 2
  And the old QR code (version 1) becomes invalid
  And I see a warning "Previous QR codes for this table will no longer work"

Scenario: Customer scans valid QR code
  Given table "Table 5" has a valid QR code
  When a customer scans the QR code with their phone
  Then they are redirected to the tenant's menu page
  And the URL includes the table context (tableId=xxx)
  And a session is created binding the customer to "Table 5"

Scenario: Customer scans expired QR code
  Given table "Table 5" had a QR code that expired 1 month ago
  When a customer scans the expired QR code
  Then they see an error page "This QR code has expired"
  And they see a message "Please ask staff for a new QR code"
  And no order can be placed
```

---

## FR-4: Customer Ordering (Core Flow)

### AC-4.1: Customer places an order via QR
**Story:** FR-4-001

```gherkin
Feature: Customer Ordering
  As a customer
  I want to scan a QR and place an order
  So that I can order without waiting for staff

Scenario: Successful order placement (happy path)
  Given I am seated at table "Table 5"
  And I scan the table's QR code
  When the menu page loads
  And I browse the menu
  And I add "Margherita Pizza" ($12.99) to my cart
  And I add "Caesar Salad" ($8.99) to my cart
  And I proceed to checkout
  And I select payment method "Card"
  And I enter my card details
  And I click "Place Order"
  Then my payment is processed successfully
  And an order is created with status "Received"
  And I see an order confirmation page with order number #12345
  And staff are notified of the new order
  And the order appears in the staff dashboard/KDS

Scenario: Order placement with modifiers
  Given I scan the QR and open the menu
  When I select "Coffee" and choose modifiers:
    | Modifier Group | Selection       |
    | Size           | Large ($4)      |
    | Extras         | Oat Milk ($0.75)|
  And I add to cart
  And I proceed to checkout and pay
  Then the order includes "Coffee" with modifiers "Large, Oat Milk"
  And the total reflects the modifier prices ($4.75)

Scenario: Order placement fails due to payment error
  Given I have items in my cart
  When I proceed to checkout
  And I enter invalid card details
  And I click "Place Order"
  Then I see an error message "Payment failed. Please check your card details."
  And the order is not created
  And I can retry with different payment details
```

### AC-4.2: Customer can edit cart before checkout
**Story:** FR-4-002

```gherkin
Feature: Cart Management
  As a customer
  I want to edit my cart before checkout
  So that I can adjust my order

Scenario: Add and remove items from cart
  Given I have added "Pizza" to my cart
  When I add "Salad" to my cart
  Then my cart shows 2 items
  When I remove "Pizza" from my cart
  Then my cart shows 1 item ("Salad")
  And the cart total updates accordingly

Scenario: Update item quantity
  Given I have added "Water" (qty: 1) to my cart
  When I increase quantity to 3
  Then my cart shows "Water x3"
  And the cart total is updated (unit price × 3)

Scenario: Clear entire cart
  Given I have 3 items in my cart
  When I click "Clear Cart"
  And I confirm the action
  Then my cart is empty
  And I can start a new order
```

### AC-4.3: Cart session persists during short visits
**Story:** FR-4-003

```gherkin
Feature: Cart Persistence
  As a customer
  I want my cart to persist if I reload the page
  So that I don't lose my order

Scenario: Cart persists on page reload
  Given I have added 2 items to my cart
  When I accidentally refresh the page
  Then my cart still contains the 2 items
  And I can continue with checkout

Scenario: Cart expires after 30 minutes of inactivity
  Given I added items to my cart 35 minutes ago
  And I have not interacted with the page
  When I return to the page
  Then I see a message "Your session has expired. Please scan the QR code again."
  And my cart is cleared
```

---

## FR-5: Payment & Checkout Options

### AC-5.1: Customer pays with card at checkout
**Story:** FR-5-001

```gherkin
Feature: Card Payment
  As a customer
  I want to pay with my card
  So that I can complete my order online

Scenario: Successful card payment
  Given I have items in my cart totaling $25.50
  When I proceed to checkout
  And I select payment method "Card"
  And I enter valid card details
  And I click "Pay Now"
  Then the payment is processed via Stripe
  And I see a success message "Payment successful"
  And the order status updates to "Paid"
  And I receive a digital receipt via email

Scenario: Payment declined by bank
  Given I proceed to checkout with total $25.50
  When I enter card details that will be declined
  And I click "Pay Now"
  Then I see an error "Your card was declined"
  And the order is not created
  And I can retry with a different card
```

### AC-5.2: Support bill-to-table option
**Story:** FR-5-002

```gherkin
Feature: Bill-to-Table Payment
  As a customer
  I want to pay at the table later
  So that I can order now and pay when ready

Scenario: Place order with bill-to-table
  Given I have items in my cart
  When I proceed to checkout
  And I select payment method "Bill to Table"
  And I click "Place Order"
  Then the order is created with status "Pending Payment"
  And I see a confirmation "Order placed! Pay at the table when ready."
  And staff see the order marked "Unpaid"
```

---

## FR-6: Order Processing & KDS

### AC-6.1: Staff view and update order states
**Story:** FR-6-001

```gherkin
Feature: Order Processing (KDS)
  As a kitchen/staff user
  I want to see incoming orders and update their status
  So that I can manage order preparation

Scenario: View new order in KDS
  Given a customer has placed an order #12345
  When I open the staff dashboard/KDS
  Then I see order #12345 with status "Received"
  And the order shows:
    | Field       | Value                   |
    | Table       | Table 5                 |
    | Items       | Pizza, Salad            |
    | Total       | $21.98                  |
    | Time        | 2 minutes ago           |

Scenario: Accept and prepare order
  Given order #12345 has status "Received"
  When I click "Accept Order"
  Then the order status updates to "Preparing"
  And a timer starts to track preparation time
  And the customer sees status "Preparing" if they check

Scenario: Mark order ready
  Given order #12345 has status "Preparing"
  When I click "Mark Ready"
  Then the order status updates to "Ready"
  And the customer receives a notification "Your order is ready!"
  And the order moves to the "Ready" section of the KDS

Scenario: Complete order
  Given order #12345 has status "Ready"
  When I click "Complete"
  Then the order status updates to "Completed"
  And the order is archived
  And metrics are updated (time-to-serve logged)
```

### AC-6.2: Order notifications
**Story:** FR-6-002

```gherkin
Feature: Real-time Order Notifications
  As a staff user
  I want to receive instant notifications for new orders
  So that I don't miss any orders

Scenario: Receive new order notification
  Given I have the staff dashboard open
  When a customer places a new order #12346
  Then I see a notification badge on the orders tab
  And I hear a notification sound (if enabled)
  And the new order appears at the top of the list

Scenario: Notification when customer updates order (future enhancement)
  Given order #12345 is in "Preparing" status
  When the customer requests a modification
  Then I see a notification "Order #12345 modified"
```

---

## FR-7: Admin Controls & Permissions

### AC-7.1: Invite staff users with roles
**Story:** FR-7-001

```gherkin
Feature: Staff User Management
  As a tenant admin
  I want to invite staff and assign roles
  So that staff can help manage the restaurant

Scenario: Invite a staff member
  Given I am logged in as tenant admin
  When I navigate to "Team Management"
  And I click "Invite Staff"
  And I enter email "chef@restaurant.com"
  And I assign role "Kitchen Staff"
  And I click "Send Invitation"
  Then an invitation email is sent to "chef@restaurant.com"
  And the user appears in the pending invitations list

Scenario: Staff member accepts invitation
  Given I have received an invitation email
  When I click the invitation link
  And I set my password
  And I accept the invitation
  Then my account is created with role "Kitchen Staff"
  And I can log in to the staff dashboard
  And I have permissions for my role only

Scenario: Role-based access control
  Given I am logged in with role "Kitchen Staff"
  When I try to access "Menu Management" (admin-only)
  Then I see an error "Access Denied"
  And I am redirected to the staff dashboard
```

---

## FR-8: Analytics & Reporting

### AC-8.1: View basic analytics
**Story:** FR-8-001

```gherkin
Feature: Analytics Dashboard
  As a tenant admin
  I want to view analytics
  So that I can measure performance

Scenario: View orders per day
  Given I am on the analytics dashboard
  When I select date range "Last 7 days"
  Then I see a chart showing orders per day
  And I see total orders: 145
  And I see average orders per day: 20.7

Scenario: View conversion rate
  Given 1000 QR scans occurred in the last 7 days
  And 120 orders were placed
  When I view the conversion metric
  Then I see conversion rate: 12%

Scenario: View average order value (AOV)
  Given 100 orders were placed with total revenue $2,500
  When I view the AOV metric
  Then I see AOV: $25.00
```

---

## FR-9: Error Handling & Support Flows

### AC-9.1: Friendly error messages for invalid QR
**Story:** FR-9-001

```gherkin
Feature: QR Code Error Handling
  As a customer
  I want to see clear error messages
  So that I know what to do if the QR is invalid

Scenario: Scan expired QR code
  Given table "Table 5" has an expired QR code
  When I scan the QR code
  Then I see a friendly error page
  And the message says "This QR code has expired"
  And I see instructions "Please ask staff for a new code"
  And I see contact info for the restaurant

Scenario: Scan QR for deactivated table
  Given table "Table 5" has been marked inactive by admin
  When I scan the QR code
  Then I see a message "This table is currently unavailable"
  And I see instructions "Please ask staff for seating assistance"
```

---

## FR-10: Admin Maintenance

### AC-10.1: Deactivate a table
**Story:** FR-10-001

```gherkin
Feature: Table Deactivation
  As a tenant admin
  I want to deactivate tables
  So that customers cannot order from unavailable tables

Scenario: Deactivate a table
  Given table "Table 5" is currently active
  When I navigate to table management
  And I select "Table 5"
  And I click "Deactivate"
  And I confirm the action
  Then "Table 5" status changes to "Inactive"
  And customers scanning the QR see "Table unavailable"
  And no new orders can be placed for "Table 5"

Scenario: Reactivate a table
  Given table "Table 5" is inactive
  When I click "Reactivate"
  Then "Table 5" status changes to "Active"
  And customers can scan and order again
```

---

## Summary & Test Coverage

### Test execution checklist
- [ ] All scenarios run in automated test suite (Playwright/Cypress for E2E)
- [ ] Manual QA sign-off for critical paths (ordering, payment, QR scan)
- [ ] Performance tests for concurrent users (load testing)
- [ ] Security tests for auth and token validation
- [ ] Accessibility tests (WCAG 2.1 AA compliance)

### Related docs
- [06-USER_STORIES.md](./06-USER_STORIES.md) — Source user stories
- [02-SRS.md](./02-SRS.md) — Functional requirements
- [15-TEMPLATES_ACCEPTANCE_TEST.md](./15-TEMPLATES_ACCEPTANCE_TEST.md) — Template for additional test cases
- [diagrams/ordering-flow.md](./diagrams/ordering-flow.md) — Visual flow for ordering scenarios

> Last updated: 2025-11-01
