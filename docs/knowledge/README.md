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
| `sync-log.md` | Which merged stories are already synced into this knowledge base | everyone |

## Who writes what, and when
| File | Written by | When |
|------|------------|------|
| `00-decisions.md` | Any agent that receives a product-wide answer | Immediately |
| `03-business-rules.md` | BA | When a story or change request is approved (ACs and tests need real `BR-xx` IDs) |
| `01`, `02`, `04`, `05`, `06` | **KB agent** | Manually, after the story's PR is merged (`/sync-knowledge <STORY-ID>`) |
| `sync-log.md` | KB agent | At each sync |

BA, DEV and QC do not edit `01`, `02`, `04`, `05`, `06`. They list proposed changes under **Knowledge impact** in `story.md` or `plan.md`.

## The gap between merge and sync
After a merge and before the sync, `01`, `02`, `04`, `05` and `06` can be out of date. The rule that covers this:
a story with Delivery status `Merged` and no row in `sync-log.md` is **unsynced**. Agents then trust that story's
`story.md` (Knowledge impact) and the merged code over those files, and remind the user to run `/sync-knowledge`.
Run `/audit-knowledge` any time to list mismatches.

## How to use and maintain
- Stories reference rules by ID, for example "AC-3 enforces BR-04".
- When the owner confirms an assumption, record it in `00-decisions.md`. KB removes the `[ASSUMPTION]` marker at the next sync.
- Never copy knowledge into a story. Link to it, so there is one place to fix.
- As-built files (`05`, `06`) follow the merged code: migrations and routes win over plans.
