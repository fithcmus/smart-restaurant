# Ordered Docs — priority list

This file lists the documentation folders in priority order. Use this as a checklist for which document types to create first.

1. product/ — One-pager, Roadmap, Vision & OKRs (MVP scope and priorities)
2. api/ — OpenAPI / API contracts (contract-first for backend/frontend)
3. architecture/ — System architecture, ER diagram, sequence diagrams
4. dev/ — Local setup, contributing guide, coding standards
5. infra/ — CI pipeline, IaC, deployment runbook
6. qa/ — Test strategy, acceptance tests, QA checklist
7. ops/ — Monitoring, on-call runbook, SLOs/SLAs
8. security/ — Threat model, data/privacy, secrets policy
9. ux/ — Personas, user journeys, wireframes, accessibility
10. user/ — Admin guide, customer FAQ, staff/KDS guide
11. analytics/ — Event schema, KPIs, instrumentation plan
12. business/ — Billing, pricing, transaction reports
13. releases/ — Release notes template and release process
14. risks/ — Risk log and feasibility analysis
15. research/ — Market analysis and competitor research
16. legal/ — Terms, privacy, compliance docs
17. maintenance/ — Tech debt register, archival/EOL plans
18. techdebt/ — Technical debt backlog and remediation plan

Notes
- If you prefer numeric prefixes on folder names (e.g. `01-product`), use the included `scripts/rename-doc-folders.ps1` to rename folders safely. Running the script is optional and will modify folder names; review the mapping inside the script before running.
- Keep this file updated if you add/remove docs folders or change priorities.

> Created: 2025-10-31
