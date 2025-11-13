# Product docs — `docs/01-product`

This folder contains the canonical product documentation for the Unified Restaurant Ordering Platform project. Files are intentionally numbered to indicate authoring and review order (lower numbers = higher priority).

How files are ordered

- 01-ONE_PAGER.md — Short problem statement and MVP summary (high-level communication).
- 02-SRS.md — Canonical Software Requirements Specification (detailed functional and non-functional requirements).
- 03-SRS_CHANGELOG.md — Changelog for the SRS (versioned edits).
- 04-MVP_SCOPE.md — Explicit in-scope and out-of-scope items for the MVP.
- 05-EPICS.md — High-level epics mapped to releases/owners.
- 06-USER_STORIES.md — Prioritized user stories derived from epics (ready to turn into tickets).
- 07-ACCEPTANCE_CRITERIA.md — Formal acceptance criteria used by QA.
- 08-METRICS_KPIS.md — Core metrics, owners, and targets.
- 09-ROADMAP.md — Release schedule and milestones.
- 10-VISION_AND_OKRS.md — Product vision and top-level OKRs.
- 11-RELEASE_CRITERIA.md — Conditions that must be met for production releases.
- 12-ADR_INDEX.md + `ADR/` — Architecture Decision Records (numbered ADRs) and index.
- 13-MEETINGS_README.md + `MEETINGS/` — Meeting notes and templates.
- 14-TEMPLATES_USER_STORY.md and 15-TEMPLATES_ACCEPTANCE_TEST.md — Reusable templates for consistency.
- `diagrams/` — Mermaid diagrams (user journeys, flows, architecture, state machines). See [diagrams/README.md](./diagrams/README.md).

Naming & contribution conventions

- File names start with a two-digit prefix to reflect ordering (01, 02, 03...). Keep this prefix when adding new top-level product docs.
- Use `TEMPLATES/` for copyable templates (user stories, acceptance tests). Create new docs by copying a template and adding a numeric prefix.
- ADRs live in `ADR/` and are numbered with the `0001-<slug>.md` pattern; add new ADR files and update `12-ADR_INDEX.md` with a link.
- Update `03-SRS_CHANGELOG.md` whenever `02-SRS.md` changes. Changes to `02-SRS.md` should be made via PR with a changelog entry.

PR / review rules

- All edits to files in this folder must be made via Pull Request.
- Include the related SRS/epic/story IDs in the PR description (e.g., `FR-4`, `EPIC-03`).
- Add reviewers: Product Owner + Tech Lead for SRS/epics; Developer + QA for user stories and acceptance criteria.

Publishing & docs site

- We publish a rendered docs site from the `docs/` folder (e.g., via MkDocs or Docusaurus). See `docs/` root for CI/pipeline instructions (not yet configured).

Quick actions

- To add a new top-level product doc:
  1. Copy the closest template in `TEMPLATES/` if applicable.
  2. Choose the next numeric prefix based on desired ordering.
  3. Create a PR referencing the SRS/EPIC and a changelog entry if it affects requirements.

Contact

- For questions about product docs structure contact: Product Owner / doc owner (add contact here).

> Created: 2025-10-31
