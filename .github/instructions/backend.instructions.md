---
applyTo: "server/**"
---

# Backend rules (Node.js, Express, SQLite)

- Layer the code: **routes** (HTTP only) -> **services** (business rules) -> **repositories** (SQL only).
  Routes never contain SQL, and repositories never contain business rules.
- Use **parameterized queries** only. Never build SQL by concatenating user input.
- Validate every request body, query and params at the route boundary, and return clear 4xx errors.
- Use one central error-handling middleware. Return a consistent error shape:
  `{ "error": { "code": "...", "message": "..." } }`.
- **Security:** every route except register, login and health check sits behind the `authenticate` middleware.
  Hash passwords with bcrypt (or argon2). Never log or return passwords or password hashes.
  Use one generic message for failed logins.
- **Soft delete:** never run `DELETE` on soft-deletable tables. Set `is_deleted = 1` instead.
  Every read query filters `is_deleted = 0` (and joins `meetings` for action items and decisions).
  Put this filter in the repository layer only.
- Use async/await. Always handle rejected promises so Express does not hang.
- Schema changes go through numbered migration files (for example `server/migrations/001_create_meetings.sql`).
  Never edit a migration that has already been applied.
- Keep configuration (port, database file path, session secret) in environment variables, not in code.
  Provide `.env.example` and never commit real secrets.
- Tests: Vitest (or Jest) + Supertest against an in-memory SQLite database.
  Each test sets up its own data and does not depend on test order.
