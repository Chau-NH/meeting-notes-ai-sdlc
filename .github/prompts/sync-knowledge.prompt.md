---
description: Sync docs/knowledge with a merged story (run manually after the PR is merged)
agent: KB
---

Sync the knowledge base with a merged story.

Input from the user: the story ID. No PR or commit is needed: read the code on `main` and find the story's commits by its ID in the commit messages.
If no story ID is given, list the stories whose Delivery status is `Merged` and that have no row in `docs/knowledge/sync-log.md`, and ask which one.

Follow the Sync process in the KB agent: read the story folder (including Knowledge impact), compare it with the merged code and migrations,
make the smallest edits needed in the knowledge files you own, add a row to `sync-log.md`, and report what you updated plus any findings.
Do not change `00-decisions.md`, `03-business-rules.md`, code, stories, plans or tests.
