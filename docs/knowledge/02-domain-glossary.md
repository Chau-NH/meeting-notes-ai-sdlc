# 02 Domain glossary

Use these exact terms in stories, code, database names, tests and UI text.

| Term | Definition | Code / DB name |
|------|------------|----------------|
| Workspace | The single shared space where all signed-in users see the same meetings | n/a |
| User | A person with an account (name, email, password). Users are the attendees and owners | `user`, table `users` |
| Meeting | A recorded session with a title, a date, attendees and notes | `meeting`, table `meetings` |
| Notes | Free text written manually during or after a meeting | `notes` column on `meetings` |
| Attendee | A user who took part in a meeting | table `meeting_attendees` |
| Action item | A follow-up created in a meeting, with an owner, due date and status | `actionItem`, table `action_items` |
| Owner | The user responsible for an action item (need not be an attendee) | `owner_id` |
| Due date | The date by which an action item should be done | `due_date` |
| Status | Progress of an action item: Open, In progress, Done | `status` |
| Overdue | Due date is in the past and status is not Done. **Derived, never stored** | computed |
| Unassigned | An action item with no owner | `owner_id IS NULL` |
| Decision | A conclusion agreed in a meeting, with optional context | `decision`, table `decisions` |
| Dashboard | Cross-meeting view of action items with filters | UI screen |
| Deleted (soft delete) | Hidden from the product but kept in the database, marked by `is_deleted = 1` | `is_deleted` |
| Session | The period during which a user stays logged in | auth cookie |

## Terms to avoid
- Use "action item", not "task" (task is reserved for the DEV breakdown in `task.md`).
- Use "owner", not "assignee".
- Use "user", not "person" or "member".
- In the UI say "Delete", but in code and docs remember it means soft delete (BR-60).
