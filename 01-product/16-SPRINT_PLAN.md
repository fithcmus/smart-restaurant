# Sprint Plan - Unified Restaurant Ordering Platform

**Version:** 1.0  
**Last Updated:** 2025-11-01  
**Planning Date:** 2025-11-01  
**Target Launch:** Q2 2026  
**Owner:** Product & Engineering Team

## Executive Summary

This sprint plan outlines the 9-sprint roadmap to deliver the Unified Restaurant Ordering Platform. The plan is organized into three phases:

- **Phase 1 (Sprints 1-3):** Foundation & Core Features - Auth, Menu Management, QR System
- **Phase 2 (Sprints 4-6):** Customer Ordering & Kitchen Operations - Complete order flow, payments, KDS
- **Phase 3 (Sprints 7-9):** Staff Management & Analytics - RBAC, reporting, polish

**Total Duration:** 18 weeks (assuming 2-week sprints)  
**MVP Completion:** End of Sprint 6 (Week 12)  
**Full Launch:** End of Sprint 9 (Week 18)

---

## Sprint Velocity & Capacity

### Team Structure
- **Frontend Developers:** 2 (React/Next.js)
- **Backend Developers:** 2 (NestJS/Node.js)
- **Full-Stack:** 1 (Can work both sides)
- **QA Engineer:** 1 (Manual + automation)
- **Product Owner:** 1 (Part-time)

### Velocity Planning
- **Target Velocity:** 10-13 points per sprint
- **Sprint Duration:** 2 weeks (10 working days)
- **Story Point Conversion:** 1 point ≈ 1 developer-day

---

## Phase 1: Foundation & Core Features

### Sprint 1: Authentication & Menu Basics
**Duration:** Weeks 1-2  
**Goal:** Enable tenant signup and basic menu creation  
**Total Points:** 11

#### User Stories
| ID | Story | Points | Assignee | Dependencies |
|---|---|---|---|---|
| FR-10-001 | Admin Login | 2 | Backend Dev 1 | None |
| FR-1-001 | Basic Tenant Signup | 2 | Backend Dev 1 | FR-10-001 |
| FR-1-002 | Email Verification | 2 | Backend Dev 2 | FR-1-001 |
| FR-2-001 | Create Menu Categories | 2 | Full-Stack | FR-1-002 |
| FR-2-002 | Create Menu Items | 3 | Frontend Dev 1, Backend Dev 2 | FR-2-001 |

#### Sprint Goals
- ✅ Tenant can sign up and verify email
- ✅ Tenant can log in and access dashboard
- ✅ Tenant can create menu categories and items
- ✅ PostgreSQL schema deployed (tenants, users, menu_items, categories)
- ✅ JWT authentication working
- ✅ Email service integrated (SendGrid/SES)

#### Key Deliverables
- [ ] Auth API endpoints (signup, login, verify)
- [ ] Menu CRUD API endpoints
- [ ] Landing page + signup flow UI
- [ ] Admin dashboard shell
- [ ] Menu management UI
- [ ] Unit tests for auth & menu services
- [ ] Postman collection for API testing

#### Risks & Mitigations
- **Risk:** Email service delays  
  **Mitigation:** Use local SMTP (Mailhog) for development
- **Risk:** Database schema changes  
  **Mitigation:** Use migrations from day 1 (TypeORM/Prisma)

#### Definition of Done
- All stories pass acceptance criteria
- Code reviewed and merged to `main`
- Deployed to staging environment
- Manual smoke test completed by QA
- Demo to stakeholders on Sprint Review (Friday Week 2)

---

### Sprint 2: Onboarding & Table Management
**Duration:** Weeks 3-4  
**Goal:** Complete tenant setup and enable QR code generation  
**Total Points:** 13

#### User Stories
| ID | Story | Points | Assignee | Dependencies |
|---|---|---|---|---|
| FR-1-003 | Tenant Onboarding Wizard | 3 | Frontend Dev 1 | FR-1-002 |
| FR-1-004 | Tenant Profile Management | 2 | Frontend Dev 2 | FR-1-003 |
| FR-2-003 | Update/Delete Menu Items | 2 | Full-Stack | FR-2-002 |
| FR-2-005 | Menu Availability Toggle | 1 | Frontend Dev 2 | FR-2-003 |
| FR-3-001 | Add Tables | 2 | Backend Dev 1 | FR-1-003 |
| FR-3-002 | Generate QR Codes | 3 | Backend Dev 2, Full-Stack | FR-3-001 |

#### Sprint Goals
- ✅ Tenant completes onboarding wizard
- ✅ Tenant can manage restaurant profile with logo upload
- ✅ Tenant can add tables and generate QR codes
- ✅ QR codes contain HMAC-signed tokens
- ✅ Redis caching implemented for menu data
- ✅ S3 integration for images and QR codes

#### Key Deliverables
- [ ] Multi-step onboarding form with validation
- [ ] Profile edit page with S3 image upload
- [ ] Table management CRUD UI
- [ ] QR code generation service (qrcode library)
- [ ] HMAC token signing/verification middleware
- [ ] Redis caching layer for menu queries
- [ ] Integration tests for QR flow

#### Risks & Mitigations
- **Risk:** S3 upload complexity  
  **Mitigation:** Use pre-signed URLs, test with LocalStack
- **Risk:** QR token security concerns  
  **Mitigation:** Security review of HMAC implementation

#### Demo Points
- Show complete flow: Signup → Wizard → Add menu → Add tables → Generate QR
- Demonstrate QR code scanning (use phone during demo)

---

### Sprint 3: Customer Menu Viewing & Modifiers
**Duration:** Weeks 5-6  
**Goal:** Enable customers to scan QR and browse menu with customization  
**Total Points:** 10

#### User Stories
| ID | Story | Points | Assignee | Dependencies |
|---|---|---|---|---|
| FR-3-003 | Download/Print QR Codes | 2 | Full-Stack | FR-3-002 |
| FR-4-001 | Scan QR and Load Menu | 3 | Frontend Dev 1, Backend Dev 1 | FR-3-002 |
| FR-4-002 | Add Items to Cart | 2 | Frontend Dev 1 | FR-4-001 |
| FR-2-004 | Menu Item Modifiers | 3 | Frontend Dev 2, Backend Dev 2 | FR-4-002 |

#### Sprint Goals
- ✅ Tenant can download QR codes as PDF
- ✅ Customer can scan QR and view menu (mobile-responsive)
- ✅ Customer can add items to cart with local storage
- ✅ Customer can select modifiers (size, toppings, etc.)
- ✅ Public API endpoints secured with token validation

#### Key Deliverables
- [ ] PDF generation service for QR codes (pdfkit)
- [ ] Public menu API with HMAC verification
- [ ] Mobile-first menu browsing UI
- [ ] Shopping cart component with local storage
- [ ] Modifier selection modal/drawer
- [ ] Price calculation with modifier adjustments
- [ ] E2E tests for ordering flow (Playwright/Cypress)

#### Risks & Mitigations
- **Risk:** Mobile UI performance on low-end devices  
  **Mitigation:** Test on real devices, optimize images
- **Risk:** Cart state loss on page refresh  
  **Mitigation:** Use localStorage with expiry

#### Demo Points
- Print QR codes and show physical printout
- Scan QR on mobile phone and browse menu
- Add items with modifiers and show cart updates

---

## Phase 2: Customer Ordering & Kitchen Operations

### Sprint 4: Checkout & Order Preparation
**Duration:** Weeks 7-8  
**Goal:** Complete checkout flow and prepare for payment integration  
**Total Points:** 6

#### User Stories
| ID | Story | Points | Assignee | Dependencies |
|---|---|---|---|---|
| FR-4-003 | Customize Items with Modifiers | 3 | Frontend Dev 1 | FR-2-004 |
| FR-4-004 | Review Cart and Checkout | 2 | Frontend Dev 2 | FR-4-003 |
| FR-4-005 | Enter Customer Name and Notes | 1 | Frontend Dev 2 | FR-4-004 |

#### Sprint Goals
- ✅ Customer can review full cart with modifiers
- ✅ Customer can add name and special instructions
- ✅ Tax calculation implemented (configurable rate)
- ✅ Order summary shows accurate totals
- ✅ Checkout UI ready for payment integration

#### Key Deliverables
- [ ] Cart review page with item editing
- [ ] Checkout form (name, notes, table auto-filled)
- [ ] Tax calculation service
- [ ] Order summary component
- [ ] Order validation (min order, max items)
- [ ] Accessibility audit (WCAG 2.1 AA)

#### Risks & Mitigations
- **Risk:** Tax calculation complexity  
  **Mitigation:** Use simple flat rate for MVP, document for future
- **Risk:** Form validation edge cases  
  **Mitigation:** Comprehensive unit tests

#### Demo Points
- Show full customer journey: Browse → Add → Customize → Review → Checkout
- Highlight mobile UX and accessibility features

---

### Sprint 5: Payment Integration & Order Submission
**Duration:** Weeks 9-10  
**Goal:** Enable secure payments and order creation  
**Total Points:** 9

#### User Stories
| ID | Story | Points | Assignee | Dependencies |
|---|---|---|---|---|
| FR-4-006 | Submit Order and Show Confirmation | 2 | Full-Stack | FR-4-005 |
| FR-5-001 | Stripe Payment Setup (Backend) | 3 | Backend Dev 1 | FR-4-006 |
| FR-5-002 | Card Payment UI | 3 | Frontend Dev 1 | FR-5-001 |
| FR-5-003 | Payment Confirmation | 1 | Frontend Dev 2 | FR-5-002 |

#### Sprint Goals
- ✅ Customer can enter card details securely (Stripe Elements)
- ✅ Payment processed via Stripe Payment Intents
- ✅ Order created in database with payment status
- ✅ Customer sees order confirmation with order number
- ✅ Stripe webhooks handle async payment events

#### Key Deliverables
- [ ] Stripe account setup (test mode)
- [ ] Payment Intent API endpoint
- [ ] Stripe Elements integration (PCI compliant)
- [ ] Order creation service
- [ ] Payment webhook handler
- [ ] Order confirmation page with status
- [ ] Payment error handling (declined cards)
- [ ] Security audit of payment flow

#### Risks & Mitigations
- **Risk:** PCI compliance concerns  
  **Mitigation:** Use Stripe Elements (no card data touches our servers)
- **Risk:** Webhook reliability  
  **Mitigation:** Implement idempotency keys and retry logic
- **Risk:** Payment delays  
  **Mitigation:** Show loading states, handle timeouts

#### Demo Points
- Complete end-to-end order with real test card (4242 4242 4242 4242)
- Show order confirmation and database record
- Demonstrate error handling (use declined test card)

---

### Sprint 6: Kitchen Display System & Order Tracking
**Duration:** Weeks 11-12  
**Goal:** Enable kitchen to receive and manage orders in real-time  
**Total Points:** 11  
**🎯 MVP COMPLETION SPRINT**

#### User Stories
| ID | Story | Points | Assignee | Dependencies |
|---|---|---|---|---|
| FR-6-001 | View New Orders | 2 | Frontend Dev 1, Backend Dev 1 | FR-4-006 |
| FR-6-002 | Update Order Status | 2 | Full-Stack | FR-6-001 |
| FR-6-003 | Real-time Order Updates | 3 | Backend Dev 2, Frontend Dev 2 | FR-6-002 |
| FR-9-001 | Track Order Status | 2 | Frontend Dev 1 | FR-6-003 |
| FR-5-004 | Payment Failure Handling | 2 | Frontend Dev 2 | FR-5-003 |

#### Sprint Goals
- ✅ Kitchen sees new orders instantly (WebSocket)
- ✅ Kitchen can update order status (preparing → ready → delivered)
- ✅ Customer sees live order status updates
- ✅ Sound/visual alerts for new orders
- ✅ Payment failure retry flow completed
- ✅ **ALL P0 FEATURES COMPLETE - MVP READY**

#### Key Deliverables
- [ ] WebSocket server setup (Socket.IO)
- [ ] KDS dashboard UI (optimized for kitchen tablets)
- [ ] Order status state machine
- [ ] Real-time updates on customer confirmation page
- [ ] Sound alerts for new orders
- [ ] Payment retry flow with error details
- [ ] Load testing (simulate 50 concurrent orders)
- [ ] MVP feature freeze and UAT preparation

#### Risks & Mitigations
- **Risk:** WebSocket connection drops  
  **Mitigation:** Auto-reconnect logic, fallback to polling
- **Risk:** Performance under load  
  **Mitigation:** Redis pub/sub for scalability
- **Risk:** Network issues in restaurant  
  **Mitigation:** Offline-first design with sync

#### Demo Points
- **MVP Demo to Stakeholders:**
  - Customer flow: Scan QR → Order → Pay → Track
  - Kitchen flow: Receive order → Update status → Complete
  - Show real-time updates (two screens side-by-side)
- **Metrics to highlight:**
  - End-to-end order time < 2 minutes
  - Mobile-responsive on all devices
  - Payment success rate

#### Sprint 6 Success Criteria
- [ ] All P0 user stories completed (FR-1 through FR-6, FR-9, FR-10)
- [ ] UAT completed by 3 pilot restaurants
- [ ] Performance benchmarks met (500ms API response, 1s page load)
- [ ] Security audit passed (OWASP Top 10)
- [ ] MVP launch readiness review approved
- [ ] Go/No-Go decision for production deployment

---

## Phase 3: Staff Management & Analytics

### Sprint 7: Role-Based Access & Staff Onboarding
**Duration:** Weeks 13-14  
**Goal:** Enable multi-user access with role-based permissions  
**Total Points:** 9

#### User Stories
| ID | Story | Points | Assignee | Dependencies |
|---|---|---|---|---|
| FR-7-001 | Invite Staff Members | 2 | Backend Dev 1 | FR-1-001 |
| FR-7-002 | Staff Signup via Invite | 2 | Full-Stack | FR-7-001 |
| FR-7-003 | Role-Based Access Control (RBAC) | 3 | Backend Dev 2, Full-Stack | FR-7-002 |
| FR-6-004 | Order Timer and Alerts | 2 | Frontend Dev 1 | FR-6-002 |

#### Sprint Goals
- ✅ Tenant admin can invite staff via email
- ✅ Staff can accept invite and create account
- ✅ RBAC implemented (admin, manager, kitchen, server roles)
- ✅ KDS shows elapsed time with alerts for delays
- ✅ PostgreSQL RLS policies enforce tenant isolation

#### Key Deliverables
- [ ] Staff invitation service
- [ ] Invitation email templates
- [ ] RBAC middleware and guards
- [ ] Role-based UI hiding (React context)
- [ ] PostgreSQL RLS policies
- [ ] Order timer component with alerts
- [ ] Staff management UI for admin

#### Risks & Mitigations
- **Risk:** RLS policy bugs causing data leaks  
  **Mitigation:** Comprehensive testing, security review
- **Risk:** Permission complexity  
  **Mitigation:** Start simple, iterate based on feedback

---

### Sprint 8: Analytics & Polish
**Duration:** Weeks 15-16  
**Goal:** Provide business insights and finalize P2 features  
**Total Points:** 9

#### User Stories
| ID | Story | Points | Assignee | Dependencies |
|---|---|---|---|---|
| FR-7-004 | Manage Staff Roles | 1 | Frontend Dev 1 | FR-7-003 |
| FR-8-001 | Total Revenue Metric | 2 | Backend Dev 1, Frontend Dev 2 | FR-5-001 |
| FR-8-002 | Order Count and AOV | 2 | Full-Stack | FR-8-001 |
| FR-8-003 | Popular Items Report | 2 | Backend Dev 2, Frontend Dev 2 | FR-8-002 |
| FR-3-004 | Regenerate QR Codes | 2 | Backend Dev 1 | FR-3-002 |

#### Sprint Goals
- ✅ Admin can change staff roles
- ✅ Analytics dashboard shows revenue, order count, AOV
- ✅ Popular items report helps optimize menu
- ✅ QR codes can be regenerated for security
- ✅ All P1 features complete

#### Key Deliverables
- [ ] Analytics aggregation queries (optimized)
- [ ] Analytics dashboard UI (charts with Recharts)
- [ ] Date range picker for analytics
- [ ] QR regeneration with version tracking
- [ ] Performance optimization (query caching)
- [ ] UI polish and consistency audit

---

### Sprint 9: Advanced Analytics & Launch Prep
**Duration:** Weeks 17-18  
**Goal:** Complete all features and prepare for public launch  
**Total Points:** 5  
**🚀 PUBLIC LAUNCH SPRINT**

#### User Stories
| ID | Story | Points | Assignee | Dependencies |
|---|---|---|---|---|
| FR-8-004 | Peak Hours Chart | 3 | Frontend Dev 1, Backend Dev 1 | FR-8-003 |
| - | Bug Fixes & Polish | 2 | All Team | All |

#### Sprint Goals
- ✅ Peak hours chart shows busiest times
- ✅ All critical bugs resolved
- ✅ Documentation complete (user guides, runbooks)
- ✅ Production infrastructure ready
- ✅ Support processes in place
- ✅ **PUBLIC LAUNCH APPROVED**

#### Key Deliverables
- [ ] Peak hours bar chart (hourly breakdown)
- [ ] Admin user guide
- [ ] Customer FAQ
- [ ] Operations runbook (deployment, monitoring)
- [ ] Incident response plan
- [ ] Marketing materials (landing page, demo video)
- [ ] Launch checklist completion
- [ ] Production deployment

#### Launch Checklist
- [ ] All automated tests passing (unit, integration, E2E)
- [ ] Performance benchmarks met
- [ ] Security audit completed
- [ ] Legal review (terms, privacy policy)
- [ ] Support email/chat configured
- [ ] Monitoring and alerting set up (Sentry, DataDog)
- [ ] Backup and disaster recovery tested
- [ ] Customer onboarding materials ready
- [ ] Beta customers migrated to production
- [ ] Launch announcement prepared
- [ ] Go-live approval from CEO/CTO

---

## Sprint Ceremonies & Rituals

### Daily Standups
- **Time:** 9:30 AM daily (15 minutes max)
- **Format:** What I did yesterday, what I'm doing today, blockers
- **Tool:** Zoom + Jira board

### Sprint Planning
- **Time:** Monday Week 1, 10:00 AM (2 hours)
- **Agenda:**
  1. Review previous sprint retrospective actions
  2. Present sprint goal and user stories
  3. Team estimates stories (planning poker)
  4. Assign stories to developers
  5. Identify dependencies and risks
  6. Commit to sprint backlog

### Sprint Review (Demo)
- **Time:** Friday Week 2, 3:00 PM (1 hour)
- **Audience:** Product, Engineering, Stakeholders
- **Format:** Live demo of completed stories, Q&A
- **Outcome:** Stakeholder feedback for backlog refinement

### Sprint Retrospective
- **Time:** Friday Week 2, 4:00 PM (1 hour)
- **Format:** What went well, what didn't, action items
- **Tool:** Miro board (Start, Stop, Continue)

### Backlog Refinement
- **Time:** Wednesday Week 2, 2:00 PM (1 hour)
- **Purpose:** Groom stories for next sprint, clarify acceptance criteria

---

## Dependencies & Critical Path

### Critical Path (Zero Slack)
```
FR-1-001 → FR-1-002 → FR-2-001 → FR-2-002 → FR-3-001 → FR-3-002 → 
FR-4-001 → FR-4-002 → FR-4-004 → FR-4-006 → FR-5-001 → FR-5-002 → 
FR-6-001 → FR-6-002 → FR-6-003
```

### Parallel Work Streams
- **Stream 1 (Backend):** Auth → Menus → Orders → Payments → WebSocket
- **Stream 2 (Frontend):** Signup UI → Menu UI → Ordering UI → KDS UI
- **Stream 3 (DevOps):** Infrastructure → CI/CD → Monitoring

### External Dependencies
- **Stripe Account:** Needed by Sprint 5 (Week 9)
- **AWS S3 Bucket:** Needed by Sprint 2 (Week 3)
- **SendGrid API Key:** Needed by Sprint 1 (Week 1)
- **Domain & SSL:** Needed by Sprint 6 (Week 11) for UAT

---

## Risk Register

| Risk | Probability | Impact | Mitigation | Owner |
|---|---|---|---|---|
| Stripe integration delays | Medium | High | Start setup early, use test mode | Backend Lead |
| WebSocket scaling issues | Low | High | Use Redis pub/sub, load test early | Backend Lead |
| Mobile UX problems | Medium | Medium | Test on real devices weekly | Frontend Lead |
| Database performance | Low | High | Optimize queries, add indexes | Backend Lead |
| Team member unavailability | Medium | Medium | Cross-train, document decisions | Eng Manager |
| Scope creep | High | Medium | Enforce feature freeze after Sprint 6 | Product Owner |
| Third-party service outages | Low | High | Implement fallbacks, monitoring | DevOps |

---

## Success Metrics

### Sprint Health Metrics
- **Velocity Consistency:** ±2 points sprint-to-sprint
- **Sprint Goal Success Rate:** >80% of committed stories completed
- **Story Carryover Rate:** <10% of stories carried to next sprint
- **Defect Escape Rate:** <5 critical bugs per sprint

### MVP Launch Metrics (End of Sprint 6)
- **Feature Completion:** 100% of P0 stories
- **Test Coverage:** >80% backend, >70% frontend
- **API Performance:** <500ms p95 response time
- **Page Load Time:** <1s on 4G mobile
- **Payment Success Rate:** >95%

### Business Metrics (Post-Launch)
- **Tenant Onboarding:** <20 minutes from signup to first QR code
- **Customer Order Time:** <2 minutes from scan to payment
- **Order Accuracy:** >98% (no wrong table/item)
- **Customer Satisfaction:** >4.5/5 average rating

---

## Communication Plan

### Weekly Status Report (Fridays)
- **Audience:** Stakeholders, leadership
- **Content:**
  - Sprint progress (completed/in-progress/blocked)
  - Risks and issues
  - Upcoming milestones
  - Budget/timeline status

### Monthly Steering Committee
- **Audience:** CEO, CTO, VP Product
- **Content:**
  - Phase progress
  - Budget vs. actuals
  - Go/no-go decisions
  - Roadmap adjustments

### Launch Communications
- **T-2 weeks:** Internal launch readiness email
- **T-1 week:** Beta customer notification
- **Launch day:** Press release, social media, blog post
- **T+1 week:** Launch retrospective

---

## Budget & Resources

### Development Costs (18 weeks)
- **Engineering Team:** 6 people × $100k/year = ~$103k
- **Infrastructure:** AWS + Stripe + SendGrid = ~$2k
- **Tools:** Jira + GitHub + Figma = ~$500
- **Total:** ~$105k for MVP

### Post-Launch Costs (Monthly)
- **Infrastructure:** $500-1000 (scales with usage)
- **Support:** 1 FTE = $8k/month
- **Maintenance:** 20% dev capacity = $10k/month

---

## Appendix

### Story Point Reference
- **1 point:** Toggle feature, add field, simple config change
- **2 points:** CRUD API + UI, form with validation
- **3 points:** Complex feature with integration, multi-component

### Definition of Ready (Story)
- [ ] Acceptance criteria defined
- [ ] Technical approach discussed
- [ ] Dependencies identified
- [ ] Mockups/wireframes available (if UI)
- [ ] Estimated by team

### Definition of Done (Story)
- [ ] Code complete and reviewed
- [ ] Unit tests passing (>80% coverage)
- [ ] Integration tests passing
- [ ] Acceptance criteria verified
- [ ] Deployed to staging
- [ ] Product owner accepted

### Definition of Done (Sprint)
- [ ] All committed stories done
- [ ] Sprint goal achieved
- [ ] Demo delivered
- [ ] Retrospective completed
- [ ] Next sprint planned

---

## Change Log

| Date | Version | Changes | Author |
|---|---|---|---|
| 2025-11-01 | 1.0 | Initial sprint plan created | Product Team |

---

## Approvals

| Role | Name | Signature | Date |
|---|---|---|---|
| Product Owner | _______________ | _______________ | _______________ |
| Engineering Lead | _______________ | _______________ | _______________ |
| CTO | _______________ | _______________ | _______________ |

