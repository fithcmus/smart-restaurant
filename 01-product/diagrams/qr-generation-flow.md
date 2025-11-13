# QR Code Generation & Validation Flow

This diagram shows how QR codes are generated, signed, and validated.

```mermaid
sequenceDiagram
    actor Admin as Tenant Admin
    participant Dashboard as Admin Dashboard
    participant Backend as API Server
    participant KMS as Key Management
    participant DB as Database
    participant QR as QR Generator

    Admin->>Dashboard: 1. Create table (name, capacity)
    Dashboard->>Backend: 2. POST /tenants/:id/tables
    Backend->>DB: 3. Insert table record
    DB-->>Backend: 4. Table created (ID returned)

    Backend->>KMS: 5. Get signing key (tenant-specific or global)
    KMS-->>Backend: 6. Return signing key
    Backend->>Backend: 7. Generate signed token<br/>(tenantId, tableId, expiry, version)
    Backend->>QR: 8. Generate QR PNG/SVG<br/>(URL + token)
    QR-->>Backend: 9. Return QR image data
    Backend->>DB: 10. Save token metadata (version, issued_at)
    Backend-->>Dashboard: 11. Return QR image + public URL
    Dashboard->>Admin: 12. Display printable QR

    Note over Admin,Dashboard: Admin can regenerate QR if needed
    Admin->>Dashboard: 13. Regenerate QR
    Dashboard->>Backend: 14. POST /tables/:id/qrcode
    Backend->>DB: 15. Increment token version (invalidates old token)
    Backend->>Backend: 16. Generate new signed token
    Backend->>QR: 17. Generate new QR
    Backend-->>Dashboard: 18. Return new QR + URL

    Note over QR,DB: Customer scans QR (validation flow)
    actor Customer
    Customer->>Backend: 19. GET /public/scan/:token
    Backend->>KMS: 20. Get verification key
    Backend->>Backend: 21. Verify HMAC signature
    Backend->>DB: 22. Check token version & table active
    alt Token valid & table active
        Backend-->>Customer: 23. Redirect to menu page
    else Token invalid/expired
        Backend-->>Customer: 24. Show error page (contact staff)
    end
```

## Token structure (JWT-like, HMAC-signed)
```json
{
  "t": "tenant_uuid",
  "tb": "table_uuid",
  "iat": 1698710400,
  "exp": 1730246400,
  "v": 1
}
```
- **t:** Tenant ID
- **tb:** Table ID
- **iat:** Issued at (Unix timestamp)
- **exp:** Expiry (Unix timestamp, e.g., 1 year)
- **v:** Token version (increments on regenerate; used for revocation)

## Security notes
- Signed with HMAC-SHA256 using per-tenant or platform secret.
- Token version in DB enables instant revocation without blacklist.
- Public URL pattern: `https://unifiedordering.app/s/:tenantSlug/:token`

## Related docs
- User Story: FR-3-001 (06-USER_STORIES.md)
- SRS: FR-4 QR Code Management (02-SRS.md)
- ADR: 0001-auth-strategy.md
