# Software Requirements Specification (SRS)
**For:** Multi-Tenant Restaurant Management System with QR Menu Ordering  
**Version:** 1.0  
**Date:** October 31, 2025  
**Prepared by:** Khanh Nguyen  

---

## Table of Contents

1. [Introduction](#1-introduction)  
2. [Overall Description](#2-overall-description)  
3. [Specific Requirements](#3-specific-requirements)  
4. [Appendices](#4-appendices)

---

## 1. Introduction

### 1.1 Purpose
This Software Requirements Specification (SRS) defines the requirements for the **Multi-Tenant Restaurant Management System with QR Menu Ordering**.  
The system enables restaurants to manage menus, orders, and tables while allowing customers to place orders directly by scanning QR codes.  
The platform supports multiple restaurants (tenants) within a shared infrastructure, ensuring data isolation and independent operations.

### 1.2 Document Conventions
- Requirements are labeled as **FR-x** (Functional Requirements) and **NFR-x** (Non-Functional Requirements).  
- This document follows the **IEEE Std 830-1998** format.

### 1.3 Intended Audience and Reading Suggestions
- **Developers:** Understand system architecture and logic.  
- **Project Managers:** Plan deliverables and timelines.  
- **QA Engineers:** Design test cases and verification procedures.  
- **Restaurant Owners:** Understand the features and operations.

### 1.4 Project Scope
The system allows restaurants to register as tenants, manage menus, orders, and staff, and enable customers to order via QR codes placed at each table.  
It aims to reduce errors, optimize workloads, and enhance customer satisfaction.

**Key Features:**
- Multi-tenant SaaS architecture  
- QR-based menu access and ordering  
- Real-time order tracking and notifications  
- Payment integration  
- Admin dashboards for analytics  

### 1.5 References
- IEEE Std 830-1998: *IEEE Recommended Practice for Software Requirements Specifications*  
- Documentation for ReactJS, NodeJS (NestJS), PostgreSQL  
- AWS and GCP Multi-Tenant Architecture Best Practices  

---

## 2. Overall Description

### 2.1 Product Perspective
The system is a **Software-as-a-Service (SaaS)** solution that supports multiple restaurant tenants operating within one environment.  

**Core Components:**
- **Super Admin Panel:** Manages tenants, billing, and access.  
- **Tenant Admin Dashboard:** Manages menus, tables, and staff.  
- **Customer Web App:** Accessible via QR for ordering.  
- **Kitchen Display System (KDS):** Tracks incoming orders.  

### 2.2 Product Functions
- Tenant registration and authentication  
- Menu creation and management  
- QR code generation per table  
- Order placement and tracking  
- Payment processing and reporting  
- Multi-tenant isolation and admin control  

### 2.3 User Classes and Characteristics
| User Class | Description | Technical Skill |
|-------------|--------------|----------------|
| Super Admin | Manages tenants and platform | High |
| Tenant Admin | Manages restaurant operations | Medium |
| Staff (Waiter/Kitchen) | Handles orders | Low |
| Customer | Scans QR and orders | Low |

### 2.4 Operating Environment
- **Frontend:** ReactJS / NextJS  
- **Backend:** NodeJS (NestJS)  
- **Database:** PostgreSQL or MongoDB  
- **Hosting:** AWS / GCP / Azure  
- **Devices:** Mobile, Tablet, Desktop  

### 2.5 Design and Implementation Constraints
- Multi-tenant isolation by schema or row-level security  
- JWT/OAuth2 authentication  
- GDPR-compliant data handling  
- Responsive design  

### 2.6 User Documentation
- Online Help Center  
- Admin Guide and Setup Wizard  
- Video Tutorials  

### 2.7 Assumptions and Dependencies
- Internet connectivity is required.  
- Payment APIs (Stripe, PayPal, Momo) are available.  
- QR codes are correctly deployed on tables.  

---

## 3. Specific Requirements

### 3.1 Functional Requirements

#### FR-1 Tenant Management
- **FR-1.1:** The system shall allow restaurants to register as tenants.  
- **FR-1.2:** Each tenant’s data shall be isolated.  
- **FR-1.3:** Super Admin can manage or deactivate tenant accounts.  

#### FR-2 Authentication and Authorization
- **FR-2.1:** JWT-based authentication for all users.  
- **FR-2.2:** Role-based access (Admin, Staff, Customer).  

#### FR-3 Menu Management
- **FR-3.1:** Tenant Admin can add, update, or delete menu items.  
- **FR-3.2:** Menu items shall include name, image, price, and availability.  

#### FR-4 QR Code Management
- **FR-4.1:** A unique QR code shall be generated for each table.  
- **FR-4.2:** QR codes link to a restaurant-specific customer ordering page.  

#### FR-5 Customer Ordering
- **FR-5.1:** Customers shall access the menu via QR code.  
- **FR-5.2:** Customers can add items to cart and place orders.  
- **FR-5.3:** Customers can request staff or checkout online.  

#### FR-6 Order Processing
- **FR-6.1:** Orders are displayed on the KDS in real-time.  
- **FR-6.2:** Staff can update order status.  
- **FR-6.3:** Customers receive live updates.  

#### FR-7 Payment and Billing
- **FR-7.1:** Integration with Stripe, PayPal, or local gateways.  
- **FR-7.2:** Tenant Admins can access transaction reports.  

#### FR-8 Reporting and Analytics
- **FR-8.1:** The system shall provide sales and performance reports.  
- **FR-8.2:** Super Admin can view tenant-level analytics.  

---

### 3.2 Non-Functional Requirements

| ID | Category | Description |
|----|-----------|-------------|
| **NFR-1** | Performance | System supports at least 1000 concurrent users per tenant. |
| **NFR-2** | Security | HTTPS, JWT, and tenant data isolation required. |
| **NFR-3** | Availability | System uptime must exceed 99.9%. |
| **NFR-4** | Scalability | Supports auto-scaling for high traffic. |
| **NFR-5** | Usability | Simple UX allowing ordering within 3 interactions. |
| **NFR-6** | Maintainability | Modular, microservice-based architecture. |
| **NFR-7** | Localization | Multi-language and multi-currency support. |

---

### 3.3 External Interface Requirements

| Interface Type | Description |
|----------------|-------------|
| **Web UI** | Responsive dashboard and menu pages. |
| **API** | RESTful API for integrations (POS, CRM). |
| **Payment Gateway** | Integration with Stripe, PayPal, Momo. |
| **Notification Service** | Email/SMS or push notifications. |

---

### 3.4 System Features
1. Multi-tenant SaaS model with data isolation.  
2. QR-based table ordering and live updates.  
3. Role-based access for Admin, Staff, and Customer.  
4. WebSocket or Socket.IO for real-time updates.  
5. Integrated reporting and analytics dashboard.  

---

## 4. Appendices

### 4.1 Glossary
- **Tenant:** Independent restaurant using the system.  
- **QR Ordering:** Digital ordering process via scanning QR code.  
- **KDS:** Kitchen Display System.  
- **SaaS:** Software as a Service.  

### 4.2 Future Enhancements
- AI-powered sales predictions.  
- Integration with delivery platforms (GrabFood, ShopeeFood).  
- Voice or chatbot ordering assistant.  
- Loyalty and rewards module.  

---


