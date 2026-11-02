---
description: Check docs/knowledge against merged code and approved stories and report mismatches (no edits)
agent: KB
---

Audit the knowledge base. Report only; do not edit any file unless the user asks afterwards.

Compare every file in `docs/knowledge/` with the merged code, migrations, routes and the approved stories.
List: outdated or missing items, items documented but not built, contradictions between knowledge files, broken `BR-xx` references,
and stories with Delivery status `Merged` that have no row in `sync-log.md`.
End with a short list in priority order and say which ones `/sync-knowledge` would fix.
