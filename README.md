# Meeting Notes Tracker

Capture what happened in meetings and make sure follow-up happens. Write meeting notes, record
decisions, assign action items to people, and track them until done.

> **Status:** project setup and knowledge base only. Application code is not created yet; it will be
> built story by story with the AI-assisted workflow described below.

## Tech stack

| Layer | Technology |
|-------|------------|
| Frontend (`client/`) | React, Vite, Tailwind CSS, DaisyUI |
| Backend (`server/`) | Node.js, Express |
| Database | SQLite |
| Authentication | Email + password, httpOnly session cookie |
| Tests (suggested, change if needed) | Vitest, React Testing Library, Supertest, Playwright |

## What the MVP includes

- Register, log in and log out (every feature requires login)
- Meetings with title, date, attendees and manually written notes
- Action items with owner, due date and status (Open, In progress, Done)
- Decisions recorded per meeting
- Dashboard of action items with filters: status, owner, overdue
- Search meetings by title and notes
- Deleting is logical only (rows are flagged `is_deleted`, never removed)

Out of scope for now: AI extraction of action items, audio and speech-to-text, email reminders, calendar integration.
Full details are in the [knowledge base](docs/knowledge/README.md).

## How we work: AI-assisted SDLC

Three custom GitHub Copilot agents cover the delivery cycle (BA, DEV, QC). A human approves at every gate.
A fourth agent, **KB**, keeps the knowledge base up to date after a story is merged. You run it manually.

```
Idea
  |
  v
BA   -> story.md (user story + Acceptance Criteria)          [approve ACs]
  |
  v
DEV  -> plan.md (how to build it)                            [approve plan]
     -> task.md (small tasks, each linked to an AC)          [approve tasks]
     -> code + unit tests                                    [review PR]
  |
  v
QC   -> test-cases.md (from ACs, can start once ACs are approved)
     -> automation tests in tests/ + results                 [approve]
```

| Agent | Responsibility | Writes |
|-------|----------------|--------|
| **BA** | Clarify requirements, write user stories and testable ACs | `story.md`, `docs/knowledge/03-business-rules.md` |
| **DEV** | Plan first, then break down tasks, then implement with unit tests | `plan.md`, `task.md`, code, migrations |
| **QC** | Design test cases from ACs, generate automation tests, report defects | `test-cases.md`, files in `tests/` |
| **KB** | After merge, sync the knowledge base with what was built; audit it on request | `docs/knowledge/` (except `00` and `03`), `sync-log.md` |

Agent definitions are in [`.github/agents/`](.github/agents). Handoff buttons at the end of each
agent response move the work to the next agent.

### Working on a story

1. Create the folder `docs/stories/<STORY-ID>/` (for example `MNT-001`).
2. Select the **BA** agent in Copilot Chat, describe the idea. BA creates `story.md` from
   [`docs/templates/story.md`](docs/templates/story.md) and lists Open Questions. Review and approve the ACs.
3. Use the handoff to **DEV**. DEV creates `plan.md` and stops. Approve it.
4. DEV then creates `task.md` and stops. Approve it.
5. DEV implements the tasks in order, updating each task's status and running tests.
6. Use the handoff to **QC** to finish `test-cases.md` and generate automation tests. QC can already
   draft test cases after step 2, in parallel with DEV.
7. Open a PR. Merge after human review. DEV sets the story's **Delivery status** to `Merged` in the last commit.
8. After the merge, run `/sync-knowledge <STORY-ID>` (agent **KB**) to bring the knowledge base up to date. KB reads the code on `main` and finds the story's commits by the story ID in the commit messages and PR title, so no PR link is needed. Run `/audit-knowledge` any time to check for mismatches.

If an agent finds an unclear or untestable requirement, it hands the work back instead of guessing.

## Changing a requirement

When an approved AC changes, nobody redoes the whole story. The change is handled in five steps:

1. Run `/change-requirement` (or select **BA**) and describe the change. BA creates `changes/CR-<nn>.md`.
2. BA finds the impact: everything linked to the AC ID, everything sharing the same business rule, table, endpoint or file, and the knowledge files.
3. You approve the **impact list**. Anything not on it is declared unaffected and stays untouched.
4. DEV updates only the listed tasks (using small delta tasks) and QC updates only the listed test cases and tests.
5. The **full** test suite runs. A failure outside the impact list means something was missed, so the CR is extended.

If someone edits `story.md` directly without a CR, DEV and QC notice it (each document records the **Story baseline** it was built on),
stop, and `/sync-story-changes` rebuilds the missing CR from the git diff, so the work can continue with only the affected parts.

Correctness comes from three things: stable IDs and complete traceability (AC -> task -> test case -> automated test),
the shared-dependency check, and the full regression run.

## Repository layout

```
.github/
  copilot-instructions.md       Project-wide instructions for Copilot
  instructions/                 Rules by file path (frontend, backend, tests)
  agents/                       BA, DEV, QC and KB custom agents
  prompts/                      Reusable prompts (/change-requirement, /sync-story-changes, /sync-knowledge, /audit-knowledge)
docs/
  knowledge/                    Product knowledge: decisions, glossary, business rules, data model, architecture
  templates/                    story.md, plan.md, task.md, test-cases.md
  stories/<STORY-ID>/           One folder per story (story, plan, task, test cases, changes/CR-xx.md)
client/                         React app (to be created)
server/                         Express API and SQLite migrations (to be created)
tests/                          API and end-to-end automation tests (to be created)
```

## Project knowledge

Start with [`docs/knowledge/README.md`](docs/knowledge/README.md).

| Document | Contents |
|----------|----------|
| [Decision log](docs/knowledge/00-decisions.md) | Confirmed decisions with dates |
| [Product overview](docs/knowledge/01-product-overview.md) | Vision, scope, open questions |
| [Glossary](docs/knowledge/02-domain-glossary.md) | Shared vocabulary |
| [Business rules](docs/knowledge/03-business-rules.md) | Numbered rules (`BR-xx`) |
| [Users and flows](docs/knowledge/04-user-roles-and-flows.md) | Personas, flows, screens |
| [Data model](docs/knowledge/05-data-model.md) | SQLite schema |
| [Architecture](docs/knowledge/06-architecture.md) | Layers, authentication, API outline |

Items marked `[ASSUMPTION]` are not confirmed yet. When one is confirmed, remove the marker and add
a line to the decision log. KB removes the marker at the next sync.

## Prerequisites

- Visual Studio Code **1.106 or later** (needed for agent handoffs)
- GitHub Copilot with Chat in **Agent** mode
- Node.js (LTS) for the application, once it is created

VS Code detects the agents in `.github/agents/` automatically. Pick **BA**, **DEV** or **QC** from the
agent dropdown in Copilot Chat.

## Getting started with the application

Setup, run and test commands will be added here by the DEV agent when the first story creates
`client/` and `server/`. Until then there is nothing to install or run.

Required environment variables (see `docs/knowledge/06-architecture.md`):

| Variable | Purpose |
|----------|---------|
| `PORT` | API port |
| `DATABASE_FILE` | Path of the SQLite database file |
| `SESSION_SECRET` | Long random secret used to sign session cookies. Never commit it |

## Conventions

Agent behaviour rules (full text in `.github/copilot-instructions.md`):
- **No repeated questions.** Agents check Resolved Questions and the decision log first, ask open questions once in a batch with proposed defaults, and record answers immediately.
- **Stay in scope.** Only what the approved story, plan and task say. Extra ideas go under "Suggestions (not in scope)".
- **No guessing.** Unclear requirements are marked `[NEEDS DECISION]` and sent back to BA, never decided by DEV or QC.
- **Small context.** Files are the memory. Read the index first, then only what is needed. One story and one task at a time. Start a new chat for a new phase.

General conventions:

- Prefer readable, straightforward code over clever or heavily optimized code.
- Every task and every test case references the Acceptance Criteria it covers (for example `AC-2`).
- Business rules are referenced by ID (for example `BR-12`), never copied.
- Delete means soft delete: set `is_deleted = 1`, never remove rows.
- Business rules (`03`) are written by BA when a story is approved. All other knowledge is synced by the KB agent after merge (`/sync-knowledge`). Agents never edit it directly.
