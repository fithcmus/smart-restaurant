# User Journey Map

High-level journey for the three primary user types.

## Customer Journey (Diner ordering at table)

```mermaid
journey
    title Customer Orders via QR
    section Arrival
      Sit at table: 5: Customer
      See QR code on table: 5: Customer
    section Ordering
      Scan QR with phone: 4: Customer
      Browse menu: 5: Customer
      Add items to cart: 5: Customer
      Customize order (modifiers): 4: Customer
      Review cart: 5: Customer
      Checkout & pay: 3: Customer
    section Waiting
      Receive confirmation: 5: Customer
      Track order status: 4: Customer
      Get "Order Ready" notification: 5: Customer
    section Completion
      Pick up / Staff delivers: 5: Customer, Staff
      Leave feedback (optional): 3: Customer
```

## Tenant Admin Journey (Restaurant owner setting up)

```mermaid
journey
    title Tenant Admin Onboarding
    section Signup
      Visit landing page: 5: Admin
      Sign up for account: 4: Admin
      Verify email: 3: Admin
    section Onboarding
      Enter restaurant details: 4: Admin
      Set operating hours: 5: Admin
      Upload logo (optional): 4: Admin
    section Menu Setup
      Create categories: 4: Admin
      Add menu items (name, price, image): 3: Admin
      Add modifiers (size, extras): 3: Admin
      Publish menu: 5: Admin
    section Table & QR Setup
      Create table records: 4: Admin
      Generate QR codes: 5: Admin
      Print QR codes: 4: Admin
      Place QR at tables: 5: Admin, Staff
    section Operations
      Monitor incoming orders: 5: Admin, Staff
      View analytics dashboard: 4: Admin
      Update menu/prices: 4: Admin
```

## Staff Journey (Kitchen/Server processing orders)

```mermaid
journey
    title Staff Order Processing
    section Order Notification
      Receive new order alert: 5: Staff
      View order details (KDS): 5: Staff
    section Preparation
      Accept order: 5: Staff
      Mark "Preparing": 5: Staff
      Prepare items: 4: Staff
      Mark "Ready": 5: Staff
    section Handoff
      Notify customer (push): 5: System
      Deliver to table (optional): 5: Staff
      Mark "Completed": 5: Staff
    section Support
      Handle customer requests: 3: Staff
      Resolve issues: 3: Staff
```

## Key touchpoints & pain points
- **Customer pain:** Long wait for staff to take order → **Solution:** QR self-ordering
- **Admin pain:** Complex POS systems → **Solution:** Simple web dashboard
- **Staff pain:** Missing/incorrect orders → **Solution:** Digital order list with clear item details

## Metrics to track per journey stage
- Customer: QR scan → order conversion (target: 10%)
- Admin: Signup → onboarding completion (target: 70% in 7 days)
- Staff: Order received → ready (target: median < 20 min)

## Related docs
- Personas: docs/09-ux/PERSONAS.md
- User Stories: 06-USER_STORIES.md
- Metrics: 08-METRICS_KPIS.md
