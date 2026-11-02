# 00 Decision log

Confirmed decisions about the product. Newest first. Anything not listed here and marked
`[ASSUMPTION]` in the other knowledge files is still unconfirmed.

| Date | Decision | Affects |
|------|----------|---------|
| 2026-11-02 | **Login is required in the MVP.** Users sign in with email and password. | BR-50 to BR-55, `users` table, `/auth` API |
| 2026-11-02 | **Notes are entered manually for now.** No AI extraction, speech-to-text, email reminders or calendar integration in the MVP. | 01 Product overview |
| 2026-11-02 | **Action item statuses:** Open, In progress, Done. Any status can change to any other. Overdue is computed from the due date and never stored. | BR-12, BR-13, BR-15 |
| 2026-11-02 | **Deletion is logical only.** Rows get a deleted flag (`is_deleted`) and are never physically removed. | BR-04, BR-60 to BR-62 |
| 2026-11-02 | **An action item's owner does not have to be a meeting attendee.** | BR-16 |
