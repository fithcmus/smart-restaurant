# System Architecture Overview

High-level architecture for the Unified Restaurant Ordering Platform multi-tenant restaurant platform.

```mermaid
graph TB
    subgraph "Client Layer"
        CustApp[Customer Web App<br/>Next.js / React]
        AdminApp[Admin Dashboard<br/>Next.js / React]
        StaffApp[Staff/KDS App<br/>Next.js / React]
    end

    subgraph "Edge/CDN"
        CDN[CloudFront / CDN<br/>Static assets, QR images]
    end

    subgraph "API Gateway"
        Gateway[API Gateway<br/>Auth, rate limiting, routing]
    end

    subgraph "Application Layer"
        AuthSvc[Auth Service<br/>JWT, sessions]
        TenantSvc[Tenant Service<br/>Multi-tenant mgmt]
        MenuSvc[Menu Service<br/>CRUD, publishing]
        OrderSvc[Order Service<br/>Order lifecycle]
        PaymentSvc[Payment Service<br/>Stripe integration]
        QRSvc[QR Service<br/>Generate, validate tokens]
        NotifSvc[Notification Service<br/>WebSocket, push, email]
    end

    subgraph "Data Layer"
        DB[(PostgreSQL<br/>Tenant data, orders, menus)]
        Cache[(Redis<br/>Sessions, rate limits, cache)]
        S3[S3 / Object Storage<br/>Images, QR PNGs]
    end

    subgraph "External Services"
        StripeAPI[Stripe API<br/>Payments]
        EmailAPI[Email Service<br/>SendGrid/SES]
        SMSAPI[SMS Service<br/>Twilio]
    end

    subgraph "Monitoring & Ops"
        Logs[CloudWatch Logs<br/>Centralized logging]
        Metrics[Prometheus/Grafana<br/>Metrics & alerts]
        Trace[Jaeger/X-Ray<br/>Distributed tracing]
    end

    CustApp -->|HTTPS| Gateway
    AdminApp -->|HTTPS| Gateway
    StaffApp -->|HTTPS| Gateway

    Gateway --> AuthSvc
    Gateway --> TenantSvc
    Gateway --> MenuSvc
    Gateway --> OrderSvc
    Gateway --> PaymentSvc
    Gateway --> QRSvc

    OrderSvc --> NotifSvc
    PaymentSvc --> NotifSvc

    AuthSvc --> DB
    TenantSvc --> DB
    MenuSvc --> DB
    OrderSvc --> DB
    QRSvc --> DB

    AuthSvc --> Cache
    OrderSvc --> Cache
    QRSvc --> Cache

    QRSvc --> S3
    MenuSvc --> S3

    PaymentSvc --> StripeAPI
    NotifSvc --> EmailAPI
    NotifSvc --> SMSAPI

    OrderSvc --> Logs
    PaymentSvc --> Logs
    AuthSvc --> Metrics
    OrderSvc --> Metrics
    OrderSvc --> Trace

    CDN --> S3

    style CustApp fill:#e1f5ff
    style AdminApp fill:#fff4e1
    style StaffApp fill:#f0ffe1
    style DB fill:#ffe1e1
    style Cache fill:#ffe1f5
    style S3 fill:#f5e1ff
```

## Architecture principles
1. **Multi-tenancy:** Tenant isolation at database row-level (tenant_id foreign key) or schema-level (future scaling option).
2. **Microservices (modular monolith initially):** Services are logically separated; can extract to separate deployments later.
3. **API-first:** All client apps consume REST APIs; WebSocket for real-time updates.
4. **Stateless services:** Auth via JWT; session state in Redis for fast invalidation.
5. **Event-driven notifications:** Order state changes trigger events to notification service (pub/sub or message queue in future).

## Technology stack (MVP)
- **Frontend:** Next.js 14 (App Router), React, TailwindCSS
- **Backend:** NestJS (Node.js), TypeScript
- **Database:** PostgreSQL 15+ with row-level security (RLS)
- **Cache:** Redis 7+
- **Storage:** AWS S3 or compatible (images, QR codes)
- **Payments:** Stripe (hosted checkout initially)
- **Real-time:** Socket.IO or native WebSocket
- **Hosting:** AWS (ECS/Fargate or Lambda for serverless option)
- **CI/CD:** GitHub Actions

## Data flow examples
- **Customer places order:** CustApp → Gateway → OrderSvc → DB (save order) → NotifSvc (notify staff via WebSocket) → StaffApp
- **Admin updates menu:** AdminApp → Gateway → MenuSvc → DB (update items) → Cache (invalidate tenant menu) → publish event
- **QR scan:** Customer scans QR → Gateway → QRSvc (validate token) → DB (check table) → redirect to CustApp with session

## Security layers
- **HTTPS everywhere:** TLS 1.3 at CDN/Gateway
- **JWT auth:** Short-lived access tokens (15 min); refresh tokens (7 days)
- **HMAC-signed QR tokens:** Prevents tampering; versioned for revocation
- **Rate limiting:** Per tenant/IP at Gateway layer (Redis-backed)
- **Tenant isolation:** All queries scoped by tenant_id; RLS policies enforce isolation
- **Secrets management:** AWS Secrets Manager or Vault for API keys

## Scalability & performance targets
- **Throughput:** Support 1000 concurrent customers per tenant (orders/sec TBD)
- **Latency:** API P95 < 200ms; menu load < 1s; order creation < 500ms
- **Availability:** 99.9% uptime (multi-AZ deployment)
- **Auto-scaling:** Horizontal scaling for API services based on CPU/memory

## Future enhancements
- **Message queue:** Add SQS/RabbitMQ for async order processing and event sourcing
- **CDN for dynamic content:** Cache tenant menus at edge (invalidate on publish)
- **GraphQL gateway:** Optional GraphQL layer for flexible client queries
- **Serverless functions:** Move lightweight endpoints (QR validation) to Lambda for cost optimization

## Related docs
- ADR: 0001-auth-strategy.md (multi-tenant auth)
- ER Diagram: docs/03-architecture/ER_DIAGRAM.md
- Deployment: docs/05-infra/DEPLOYMENT_RUNBOOK.md
- API Spec: docs/02-api/openapi.yaml
