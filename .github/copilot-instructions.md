# Project instructions: Meeting Notes Tracker

Meeting Notes Tracker captures meeting notes or transcripts, extracts action items
and decisions (owner, deadline, status), and tracks follow-up.

## Project knowledge (read first)
The source of truth about the product is in `docs/knowledge/` (start with `README.md`):
product overview, glossary, business rules (`BR-xx`), user flows, data model and architecture.
- Use the exact terms from `docs/knowledge/02-domain-glossary.md` in stories, code, tests and UI text.
- Reference rules by ID (for example `BR-12`) instead of copying them.
- **Who writes what:** BA writes `03-business-rules.md` and `00-decisions.md` (rules are needed with real IDs while the story is in progress).
  The **KB agent** writes `01`, `02`, `04`, `05`, `06` and `sync-log.md`, manually, after a story is merged (`/sync-knowledge`).
  BA, DEV and QC do not edit those files. They list proposed changes under **Knowledge impact** in `story.md` or `plan.md`.
- **Unsynced stories:** a story with Delivery status `Merged` and no row in `docs/knowledge/sync-log.md` is not in the knowledge base yet.
  For such a story, trust its `story.md` (Knowledge impact) and the merged code over `01`, `02`, `04`, `05` and `06`, and tell the user
  "Story <ID> is merged but not synced; run /sync-knowledge". Do not sync it yourself.
- If a story is cancelled, BA removes the rules it added to `03-business-rules.md`.
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

## Rules for all agents
These four rules apply to BA, DEV and QC in every response. Role-specific details are in `.github/agents/`.

### 1. Do not re-ask answered questions
- Before asking anything, look for the answer in: this conversation, the story's **Resolved Questions**,
  `docs/knowledge/00-decisions.md`, `plan.md`, `task.md` and the other knowledge files.
- Ask only what is still open. If an earlier answer seems to no longer hold, quote it and say what conflicts.
- Ask all open questions together, once. Number them and give each a proposed default,
  so the user can reply briefly ("1 yes, 2 option B").
- Record every answer immediately: in the story's **Resolved Questions** (question, answer, date).
  If it affects the whole product, also add a line to `00-decisions.md` (and update `03-business-rules.md` if it changes a rule).
  KB reconciles the other knowledge files at the next sync.

### 2. Stay in scope
- Do only what the approved story, AC, plan and task say. Every change must trace to an AC or a task.
- Do not add features, fields, endpoints, screens, dependencies, refactors, renames, formatting changes,
  extra tests or extra documentation that were not asked for.
- If you see something worth doing, write it under **Suggestions (not in scope)** in the file you are working on
  and do not do it. The user decides.
- Only change files that belong to your role: BA -> `story.md`, change requests, `03-business-rules.md`, `00-decisions.md`;
  DEV -> `plan.md`, `task.md`, code, migrations; QC -> `test-cases.md` and `tests/`;
  KB -> the other files in `docs/knowledge/` and `sync-log.md`.
- End each report with what changed and what you deliberately did not do.

### 3. Do not guess unclear requirements
- Ask when the answer would change behaviour, an AC, data, permissions or security.
- Decide yourself only on technical details that follow the conventions and do not change behaviour
  (names, internal structure, helper functions).
- If a point is unclear: stop work on that part, ask (rule 1 format) and mark it `[NEEDS DECISION]`.
  You may continue with parts that do not depend on it.
- Never turn an assumption into a rule silently. Write it as `[ASSUMPTION]` with a proposed default and wait for approval.
- Never invent requirements to fill a gap or to make code or tests pass.

### 4. Keep context small
- Files are the memory, not the chat. At the start of work, read the files of the current story
  (`story.md`, `plan.md`, `task.md`, `test-cases.md`) to learn the current state.
- Read `docs/knowledge/README.md` first (it is the index), then only the files you need.
  Search the code for the relevant parts instead of reading the whole repository.
- Work on one story and one task at a time. Finish it and update its status before starting the next.
- Refer to things by ID (`AC-2`, `BR-12`, `T3`) instead of copying their text.
  Show only the relevant lines, not whole files.
- Keep replies short: result, open questions, next step.
- After each phase or task, save the state in the files (status, resolved questions, decisions).
  When the chat gets long, or the work moves to another agent, say so and suggest continuing in a new chat,
  using the handoff button. Nothing important should live only in the chat.
- Keep stories small (about 8 ACs or fewer). If a story is bigger, BA proposes splitting it.

## Changing an approved requirement
A change to an approved AC is handled with a **Change Request** (`docs/stories/<STORY-ID>/changes/CR-<nn>.md`,
template `docs/templates/change-request.md`), started with the prompt `/change-requirement` or the BA agent.
- AC, test case and task IDs are permanent. Never renumber or reuse them.
- BA does the impact analysis (direct trace by ID, shared dependencies, knowledge files). The user approves the impact list.
- Only the listed items are updated. Done tasks are not reopened; small **delta tasks** make the difference.
- The full automated suite is always run. A failure outside the impact list means the analysis missed something, so extend the CR.
- Commit messages and PR titles start with the story ID, then the task (and CR) IDs, for example `MNT-001 T3 (AC-2): ...` or
  `MNT-001 CR-1 T7 (AC-2): ...`. This is how KB finds a story's commits on `main` (task IDs alone repeat across stories).
- If `story.md` was edited directly without a CR, DEV and QC detect it through the **Story baseline** in their documents,
  stop, and the user runs `/sync-story-changes` to rebuild the CR from the git diff. Nobody reworks anything before the impact list is approved.

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
