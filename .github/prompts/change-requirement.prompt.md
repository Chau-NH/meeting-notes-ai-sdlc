---
description: Handle a change to an approved Acceptance Criterion with impact analysis (no full rework)
agent: BA
---

A requirement changed after approval. Handle it as a Change Request.

Input from the user: the story ID, the AC that changes (or a new/removed AC), and the new requirement.
If any of these is missing, ask once for all missing parts.

Do this, in order:
1. Create `docs/stories/<STORY-ID>/changes/CR-<nn>.md` from `docs/templates/change-request.md`. Use the next free CR number.
2. Fill in sections 1 and 2 (trigger, before/after). Keep AC IDs; give a modified AC the next version.
3. Do the impact analysis (section 3): direct trace by AC ID, shared dependencies (same `BR-xx`, table, endpoint, screen, file; other ACs that share them), and knowledge files.
   Read `task.md` and `test-cases.md`, and search `tests/` for the AC ID. List only what is really impacted, with a reason. Say clearly what is declared unaffected.
4. Check Resolved Questions and the decision log first. Ask only the open questions, once, with proposed defaults.
5. Present the impact list and stop. Wait for approval before changing `story.md` or any knowledge file.

Do not change code, tasks or tests yourself. Do not touch ACs that are not named in the change.
