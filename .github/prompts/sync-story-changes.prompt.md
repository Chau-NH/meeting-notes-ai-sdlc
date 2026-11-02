---
description: Recover when story.md was edited directly without a Change Request (rebuild the diff and the impact list)
agent: BA
---

`story.md` was changed after approval without a Change Request. Rebuild the missing record. Do not rework anything yet.

Input from the user: the story ID. The baseline is optional (commit hash or date of the last approved story).
If it is not given, use the **Story baseline** written in `plan.md`, `task.md` or `test-cases.md` (the oldest one if they differ).
If there is none, ask the user once for the last approved version.

Do this, in order:
1. Compare the baseline version of `docs/stories/<STORY-ID>/story.md` with the current one (`git diff <baseline> -- <file>`).
   Also check whether `docs/knowledge/` changed since the baseline.
2. List the differences per AC: Modified / Added / Removed. Check specially for **renumbered, reused or deleted AC IDs**.
   If IDs changed, propose a mapping old ID -> new ID and ask the user to confirm it before anything else, because tasks and tests point to the old IDs.
3. Create `docs/stories/<STORY-ID>/changes/CR-<nn>.md` from `docs/templates/change-request.md`.
   In section 1 say: "story.md was edited directly; reconstructed from the git diff". Fill in section 2 from the diff.
4. Ask the user to confirm that the reconstructed diff matches what the client really asked. Do not assume.
5. Do the impact analysis (section 3) as usual: direct trace by AC ID, shared dependencies, knowledge files. Stop and wait for approval (section 5).
6. After approval, add the missing **Change log** row and bump the AC versions in `story.md`, then use the handoffs to DEV and QC.

Do not change code, tasks or tests yourself. Do not touch ACs that did not change.
