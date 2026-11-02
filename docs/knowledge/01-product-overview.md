# 01 Product overview

## Vision
Meeting Notes Tracker helps a team **capture what happened in meetings** and **make sure follow-up
actually happens**. Notes are written once; action items and decisions are recorded against the
meeting, assigned to people, and tracked until done.

## Problem
Meeting outcomes get lost in documents and chats. Nobody can quickly answer:
"What did we decide?", "Who owns this?", "What is overdue?".

## Target users
- **Meeting organiser**: records the meeting, notes, decisions and action items.
- **Owner**: sees the action items assigned to them and updates progress.
- **Team lead**: reviews open and overdue items across meetings.

All of them are signed-in users of one shared workspace.

## MVP scope (in scope)
1. **Authentication:** register, log in, log out. Every other feature requires login.
2. Create, view, edit and delete (logically) meetings with title, date, attendees and manually written notes.
3. Add action items to a meeting: description, owner, due date, status.
4. Record decisions for a meeting.
5. Update action item status (Open, In progress, Done).
6. A dashboard of all action items with filters: status, owner, overdue.
7. Search meetings by title and notes text.

## Out of scope for MVP
Confirmed (see `00-decisions.md`):
- Automatic AI extraction of action items from notes.
- Audio recording and speech-to-text.
- Email reminders and calendar integration.

Not yet confirmed `[ASSUMPTION]`:
- Roles and per-user permissions (all signed-in users have the same rights, BR-54).
- Recurring meetings, file attachments, comments on action items.
- A "restore deleted item" screen (deleted data stays in the database but has no UI, BR-60).
- Password reset by email, social login, two-factor authentication.

## Success measures `[ASSUMPTION]`
- A meeting with 5 action items can be recorded in under 3 minutes.
- Every action item is visible in one cross-meeting list with its owner and due date.

## Open questions
- [ ] **Who can create an account?** Anyone (self-registration), or only invited/admin-created users? (This decides whether the workspace is open to anyone who can reach the site.)
- [ ] Are roles needed (for example only the creator or an admin can edit or delete a meeting)?
- [ ] Should notes be plain text or Markdown?
- [ ] Can a deleted email address be registered again, or must it stay reserved?
