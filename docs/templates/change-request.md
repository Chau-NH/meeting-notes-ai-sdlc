# CR-<nn>: <Short title>   (story <STORY-ID>)

> File location: `docs/stories/<STORY-ID>/changes/CR-<nn>.md`.
> Rule: nothing in section 6 starts before section 5 is approved.

## 1. Trigger
What changed and why (meeting, decision, defect found), who asked, date.

## 2. The change
| AC | Before | After | Type (Modified / Added / Removed) |
|----|--------|-------|-----------------------------------|
| AC-3 |      |       | Modified                          |

AC IDs never change. A modified AC becomes the next version (v1 -> v2). A removed AC stays listed as Removed.

## 3. Impact analysis
How it is found (do all three):
1. **Direct trace:** everything linked to the changed AC ID (`task.md` coverage table, `test-cases.md` traceability table, test files that mention the ID).
2. **Shared dependencies:** other things that use the same business rule (`BR-xx`), table or column, endpoint, screen or file.
   Search for them (IDs, names, file paths from `task.md`). Any other AC that shares one of them is listed too.
3. **Knowledge files:** business rules, data model, architecture, glossary, user flows.

| Artifact | ID | Impact (Update / New / Obsolete / Re-verify only) | Reason |
|----------|----|----------------------------------------------------|--------|
| Business rule | BR-12 | Update | |
| Task | T3 | Update (new delta task T7) | |
| Test case | TC-5 | Update | |
| Automated test | `tests/api/...` | Update | |
| Other AC | AC-4 | Re-verify only | shares BR-12 |

Anything not in this table is declared **unaffected** and is not touched.

## 4. Open questions
Ask once, all together, with a proposed default. Move answers to the story's Resolved Questions.

## 5. Approval
Impact list approved by: <name>, <date>

## 6. Execution
### BA
- [ ] `story.md`: AC updated (new version), Change log row added
- [ ] `03-business-rules.md` updated; other knowledge changes listed under **Knowledge impact** in `story.md`
### DEV
- [ ] Tasks marked `Rework` or `Obsolete`; **delta tasks** added for the new behaviour (tagged `CR-<nn>`)
- [ ] Unit tests updated inside those tasks
- [ ] `plan.md` (including its **Knowledge impact**) updated only if the design changed
### QC
- [ ] Test cases updated, added or marked `Obsolete` (IDs kept)
- [ ] Automated tests updated or removed accordingly
- [ ] Traceability tables updated

## 7. Verification (correctness check)
- [ ] The **full** automated test suite was run, not only the affected tests. Result:
- [ ] Every failure outside the impact table was investigated. Each one is a missed impact: add it to section 3 and fix it
- [ ] Every active AC still has at least one task, one test case and one automated test
- [ ] `03-business-rules.md` is consistent with the new behaviour. Other knowledge files are synced by KB after merge (`/sync-knowledge`)

## 8. Outcome
Closed by <name> on <date>. Notes:
