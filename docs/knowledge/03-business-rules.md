# 03 Business rules

Each rule has a stable ID. Acceptance Criteria and tests reference these IDs.
Rules marked `[ASSUMPTION]` need confirmation. Rules without a marker are confirmed
(see `00-decisions.md`) or are plain technical constraints.

## Meetings
| ID | Rule |
|----|------|
| BR-01 | A meeting must have a title (1 to 200 characters) and a date. |
| BR-02 | Notes are optional and are entered manually. |
| BR-03 | A meeting date may be in the past or the future. `[ASSUMPTION]` |
| BR-04 | Deleting a meeting is a **soft delete** (BR-60). The UI asks for confirmation first. The meeting's action items and decisions disappear with it (BR-61). |

## Action items
| ID | Rule |
|----|------|
| BR-10 | An action item belongs to exactly one meeting and must have a description (1 to 500 characters). |
| BR-11 | Owner and due date are optional. An item without an owner is shown as **Unassigned**. |
| BR-12 | Allowed statuses are `Open`, `In progress`, `Done`. A new action item starts as `Open`. |
| BR-13 | Any status may change to any other status (no forced order). |
| BR-14 | When status becomes `Done`, `completed_at` is set. When it leaves `Done`, `completed_at` is cleared. |
| BR-15 | An action item is **overdue** when `due_date` is before today and status is not `Done`. Overdue is computed, never stored. |
| BR-16 | The owner of an action item does not have to be an attendee of the meeting. |

## Decisions
| ID | Rule |
|----|------|
| BR-20 | A decision belongs to exactly one meeting and must have a summary (1 to 500 characters). Context is optional. |

## Users
| ID | Rule |
|----|------|
| BR-30 | A user has a name (1 to 100 characters), an email and a password. All are required. |
| BR-31 | Email must be valid and unique across users (including deleted users). Compare emails case-insensitively. `[ASSUMPTION]` |
| BR-32 | Users are never physically deleted. A deleted user (`is_deleted = 1`) cannot log in and cannot be chosen as a new attendee or owner, but their name still shows on existing meetings and action items. There is no user-deletion screen in the MVP. `[ASSUMPTION]` |

## Dates and text
| ID | Rule |
|----|------|
| BR-40 | Dates are stored as ISO 8601 strings (`YYYY-MM-DD` for dates, UTC timestamps for audit fields). |
| BR-41 | Text input is trimmed. Empty strings after trimming count as missing. |

## Authentication and access
| ID | Rule |
|----|------|
| BR-50 | Every feature and every API endpoint requires a logged-in user, except register, login and the health check. Unauthenticated requests get `401`. |
| BR-51 | Registration requires name, a valid email, and a password of at least 8 characters. `[ASSUMPTION: open self-registration, see open questions]` |
| BR-52 | Passwords are stored only as a salted hash and are never returned by the API or written to logs. |
| BR-53 | A failed login shows one generic message ("Email or password is incorrect") whether the email exists or not. |
| BR-54 | All logged-in users share one workspace and have the same permissions. There are no roles in the MVP. `[ASSUMPTION]` |
| BR-55 | A session expires after 8 hours and the user must log in again. `[ASSUMPTION]` |

## Soft delete
| ID | Rule |
|----|------|
| BR-60 | "Delete" never removes a row. It sets `is_deleted = 1`. This applies to meetings, action items, decisions and users. |
| BR-61 | Every read excludes rows with `is_deleted = 1`, and also excludes the action items and decisions of a deleted meeting. Children are hidden through their parent; they are not flagged one by one. |
| BR-62 | A deleted row cannot be read or changed through the API (it behaves as `404 Not Found`). |
