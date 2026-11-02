# Tasks for <STORY-ID>

> **Story baseline:** <commit hash or date of the approved `story.md` this document is based on>

> Created by DEV after `plan.md` is approved. Each task is small, verifiable and traceable to an AC.

## Task list
| ID | Title | Covers AC | Depends on | Status | CR |
|----|-------|-----------|------------|--------|----|
| T1 |       | AC-1      | -          | Todo   |    |
| T2 |       | AC-2      | T1         | Todo   |    |

Status values: `Todo`, `In progress`, `Done`, `Blocked`, `Rework` (affected by a change request), `Obsolete` (no longer needed).

When a requirement changes after a task is `Done`, **do not reopen the task**. Mark it `Obsolete` or `Rework` only if the
change request says so, and add a small **delta task** (for example `T7`, CR column `CR-1`) that makes only the difference.
Tasks that are not in the change request's impact table stay `Done` and are not touched.

## Task details

### T1: <Title>
- **Covers AC:** AC-1
- **Depends on:** -
- **Description:** <What to change and why, in 2-4 sentences>
- **Files / components:** <expected files or modules>
- **Done when:**
  - [ ] <observable condition 1>
  - [ ] <observable condition 2>
- **Tests:** <unit tests to add or update>
- **Notes / risks:**

### T2: <Title>
- **Covers AC:** AC-2
- **Depends on:** T1
- **Description:**
- **Files / components:**
- **Done when:**
  - [ ]
- **Tests:**
- **Notes / risks:**

## AC coverage
| AC | Tasks |
|----|-------|
| AC-1 | T1 |
| AC-2 | T2 |

> Every AC from `story.md` must appear here. An AC with no task means the breakdown is incomplete.

## Suggestions (not in scope)
Improvements noticed during implementation. Not done unless the user adds them.
-

## Status
Draft | Approved by <name> on <date>
