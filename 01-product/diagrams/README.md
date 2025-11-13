# Product Diagrams Index

This folder contains Mermaid-based diagrams for the Unified Restaurant Ordering Platform. All diagrams are version-controlled as markdown with embedded Mermaid code blocks (renders automatically on GitHub, MkDocs, and most modern viewers).

## Available diagrams

### Flow & sequence diagrams
1. **[ordering-flow.md](./ordering-flow.md)** — End-to-end customer ordering sequence (QR scan → order confirmation)
2. **[qr-generation-flow.md](./qr-generation-flow.md)** — QR code generation, signing, and validation flow
3. **[order-state-machine.md](./order-state-machine.md)** — Order lifecycle state machine with valid transitions

### Journey & experience
4. **[user-journeys.md](./user-journeys.md)** — Journey maps for Customer, Tenant Admin, and Staff users

### Architecture
5. **[system-architecture.md](./system-architecture.md)** — High-level system architecture (services, data layer, external integrations)

## How to view diagrams
- **GitHub:** Mermaid renders natively in markdown preview.
- **VS Code:** Install the "Markdown Preview Mermaid Support" extension.
- **MkDocs:** Use the `mkdocs-mermaid2-plugin` (add to mkdocs.yml).
- **Docusaurus:** Mermaid is supported via `@docusaurus/theme-mermaid`.
- **Export to PNG/SVG:** Use [Mermaid Live Editor](https://mermaid.live) or CLI tools for static exports.

## Diagram conventions
- Use **Mermaid** syntax for maintainability (text-based, version-controlled).
- Include a "Related docs" section at the end of each diagram linking to SRS, user stories, ADRs.
- Update diagrams whenever requirements change (treat as code; review in PRs).

## Adding new diagrams
1. Create a new `.md` file in this folder with embedded Mermaid code.
2. Add an entry to this index.
3. Link from relevant product docs (ONE_PAGER, SRS, USER_STORIES).
4. Submit via PR with diagram explanation in the description.

## Future diagram candidates
- ER diagram (database schema) → should live in `docs/03-architecture/ER_DIAGRAM.md`
- Payment flow (Stripe integration)
- Tenant onboarding wizard flow
- Multi-location management (future feature)

## Tools & resources
- [Mermaid documentation](https://mermaid.js.org/)
- [Mermaid Live Editor](https://mermaid.live)
- [GitHub Mermaid support](https://github.blog/2022-02-14-include-diagrams-markdown-files-mermaid/)

> Created: 2025-11-01
