# 04 User roles and key flows

> Login is required (BR-50). All signed-in users have the same permissions (BR-54 `[ASSUMPTION]`).
> The personas below describe **intent**, which matters for screens and wording, not access rights.

## Personas
| Persona | Goal | Typical actions |
|---------|------|-----------------|
| Organiser | Capture the meeting fast | Create meeting, write notes, add action items and decisions |
| Owner | Know what I must do | Open dashboard, filter by my name, update status |
| Team lead | See what is stuck | Filter overdue and unassigned items, review decisions |

## Flow F0: Register and sign in
1. A visitor opens the app and is redirected to **Login** (BR-50).
2. New users choose **Register**, enter name, email and password (BR-51), and are signed in.
3. Returning users enter email and password. A wrong combination shows the generic message (BR-53).
4. After login the user lands on the Meetings list. **Log out** is always available in the header.
5. When the session expires (BR-55), the next action redirects to Login and returns the user to where they were.

## Flow F1: Record a meeting
1. Organiser opens "New meeting" and enters title, date, attendees, notes.
2. Organiser saves. The meeting detail page opens.
3. On the detail page, Organiser adds action items (description, owner, due date) and decisions.
4. Each item appears immediately in the meeting and on the dashboard.

## Flow F2: Update my action items
1. Owner opens the dashboard and filters by their name and status `Open` or `In progress`.
2. Owner changes status from the list (inline) or from the meeting page.
3. Item moves to `Done`; `completed_at` is set (BR-14).

## Flow F3: Review what is overdue
1. Team lead opens the dashboard and selects the **Overdue** filter (BR-15).
2. List shows owner, due date, meeting of origin. Lead can open the meeting from each row.
3. Lead reassigns or changes due dates where needed.

## Flow F4: Find a past decision
1. User searches by keyword. Results list meetings whose title or notes match.
2. User opens the meeting and reads its decisions.

## Flow F5: Delete a meeting
1. User chooses **Delete** on a meeting and confirms in a modal (BR-04).
2. The meeting disappears from lists, search and the dashboard, together with its action items and decisions (BR-61).
3. Data stays in the database (BR-60). There is no restore screen in the MVP.

## Screens (draft)
| Screen | Purpose |
|--------|---------|
| Login / Register | Authenticate (public) |
| Meetings list | Browse and search meetings, newest first |
| Meeting detail | Notes, attendees, action items, decisions |
| Meeting form | Create or edit a meeting |
| Dashboard | Cross-meeting action items with filters |

All screens except Login and Register are protected. The header shows the current user and **Log out**.

## Empty, loading and error states
Every list screen must show a friendly empty state, a loading indicator and a retryable error message.
