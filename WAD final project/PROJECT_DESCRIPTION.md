# Smart Restaurant - Project Description

## System Overview

**Smart Restaurant** is a QR-based menu ordering system for **dine-in service** that enables restaurants to:
- Manage digital menus with categories, items, and modifiers
- Generate unique QR codes for each table
- Allow customers to scan QR, browse menu, and place orders from their phones
- Customers can place multiple orders during their visit
- Process payments after the meal via Stripe integration (pay-after-meal model)
- Track orders in real-time via Kitchen Display System (KDS)
- View analytics and performance reports

## Technology Stack

| Layer | Technology |
| :---- | :---- |
| **Architecture** | Single Page Application (SPA) |
| **Frontend** | ReactJS / NextJS |
| **Backend** | NodeJS with Express/similar framework |
| **Database** | SQL or NoSQL database |
| **Authentication** | Passport.js with JWT |
| **Payment** | Stripe API |
| **Real-time** | Socket.IO / WebSocket |
| **Caching** | Redis (optional) |
| **Hosting** | Public hosting service |

## Key User Flows

### 1. Restaurant Setup (Admin)
```
Super Admin creates Admin account → Admin login → Menu Creation → Table Setup → QR Generation
```

### 2. Customer Registration
```
Sign up → Email Verification → Login → Access order history and preferences
```

### 3. Customer Ordering (Dine-in Only)
```
Scan QR → View Menu → Add to Cart → Customize with Modifiers → Place Order → Track Order → Request Bill → Payment
```

### 4. Order Processing
```
New Order → Accept → Preparing → Ready → Completed
```

## User Roles

| Role | Description |
| :---- | :---- |
| **Guest** | Customer who scans QR code to browse menu and place orders |
| **Customer** | Registered user with order history and saved preferences (can sign up) |
| **Super Admin** | System administrator who creates and manages Admin accounts |
| **Admin** | Restaurant owner with full access to restaurant features (created by Super Admin) |
| **Kitchen Staff** | Access to KDS for order preparation (created by Admin) |

## Account Management Flow

```
Super Admin → Creates Admin accounts → Admin manages their restaurant
                                     → Admin creates Kitchen Staff accounts
Customer → Self sign-up via registration form
```

**Note:** This is a single-restaurant system. Multi-tenant support for multiple restaurants is not included in this version.

## Core Features

### Customer-Facing (Dine-in)
- QR code menu access (table-linked)
- Menu browsing with filters and search
- Shopping cart with modifiers
- Place multiple orders per visit
- Real-time order tracking
- Request bill when ready
- Pay after meal (Stripe)
- Item reviews

### Super Admin
- Create and manage Admin accounts
- System-level configuration

### Restaurant Admin (Owner)
- Menu management (categories, items, modifiers)
- Table and QR code management
- Order management with KDS
- Revenue reports and analytics
- Create Kitchen Staff accounts

### Advanced
- Kitchen Display System (KDS) with timers
- Real-time WebSocket updates
- Fuzzy search for menu items
- Multi-language support (EN/VI)
- Redis caching
- Docker containerization
- CI/CD pipeline
