# Project instructions: Meeting Notes Tracker

Meeting Notes Tracker captures meeting notes or transcripts, extracts action items
and decisions (owner, deadline, status), and tracks follow-up.

## Project knowledge (read first)
The source of truth about the product is in `docs/knowledge/` (start with `README.md`):
product overview, glossary, business rules (`BR-xx`), user flows, data model and architecture.
- Use the exact terms from `docs/knowledge/02-domain-glossary.md` in stories, code, tests and UI text.
- Reference rules by ID (for example `BR-12`) instead of copying them.
- If you change a rule, the schema or the API, update the matching knowledge file in the same story.
- Items marked `[ASSUMPTION]` are not confirmed. Raise them as Open Questions, do not build on them silently.

## Working agreement (AI SDLC)
We work in three roles, each with its own custom agent: **BA -> DEV -> QC**.
- Every story lives in `docs/stories/<STORY-ID>/` and is handed off through Markdown files:
  `story.md` (BA), `plan.md` then `task.md` (DEV), `test-cases.md` (QC).
- DEV works in order: plan first (`plan.md`), then task breakdown (`task.md`), then implementation.
- Every task and every test case must reference the Acceptance Criteria it covers (for example `AC-2`).
- If something is unclear, write it under **Open Questions**. Do not guess.
- A human approves, in this order: the ACs (after BA), the plan, the task list (both after DEV),
  and the PR (before merge).

## Tech stack
- Frontend: React, Vite, Tailwind CSS, DaisyUI (folder `client/`)
- Backend: Node.js, Express (folder `server/`)
- Database: SQLite
- Authentication: email + password, httpOnly session cookie (see `docs/knowledge/06-architecture.md`)
- Language-specific rules are in `.github/instructions/` and apply automatically by file path.

## Coding conventions (all code)
- Prefer readable, straightforward code over clever or heavily optimized expressions.
- Small functions with descriptive names. No abbreviations in identifiers.
- Add or update unit tests with every behaviour change.
- Do not add a new dependency without saying why in `plan.md` or the PR description.

## Testing conventions
- Unit tests live next to the code they test. Automation tests (API and end-to-end) live in `tests/`.
- Test names describe behaviour: `should_<expected result>_when_<condition>`.
- Test tools (change here if your team uses others):
  - Frontend unit/component: Vitest + React Testing Library
  - Backend unit/API: Vitest (or Jest) + Supertest, against an in-memory SQLite database
  - End-to-end: Playwright
- Detailed rules for files under `tests/` are in `.github/instructions/tests.instructions.md`.
