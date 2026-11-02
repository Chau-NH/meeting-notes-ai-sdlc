# Project knowledge base: Meeting Notes Tracker

> **Status: DRAFT, partly confirmed.** Confirmed decisions are in `00-decisions.md`.
> Items marked `[ASSUMPTION]` or listed under *Open questions* still need a human to confirm them;
> then remove the marker and add a line to the decision log.

This folder is the single source of truth about **what the product is and how it works**.
All agents (BA, DEV, QC) read it before working on a story.

| File | Purpose | Main reader |
|------|---------|-------------|
| `00-decisions.md` | Confirmed decisions with dates | everyone |
| `01-product-overview.md` | Vision, users, MVP scope, out of scope, open questions | BA |
| `02-domain-glossary.md` | Shared vocabulary | everyone |
| `03-business-rules.md` | Numbered business rules (`BR-xx`) | BA, DEV, QC |
| `04-user-roles-and-flows.md` | Personas, key user flows, screens | BA, QC |
| `05-data-model.md` | SQLite tables, relationships, enums | DEV, QC |
| `06-architecture.md` | Structure, layers, auth, API outline, conventions | DEV, QC |

## How to use and maintain
- Stories reference rules by ID, for example "AC-3 enforces BR-04".
- If a story changes a rule, the **BA agent updates `03-business-rules.md` in the same story**.
- If a story changes the schema or API, the **DEV agent updates `05-data-model.md` / `06-architecture.md`**.
- When the owner confirms an assumption, record it in `00-decisions.md`.
- Never copy knowledge into a story. Link to it, so there is one place to fix.
