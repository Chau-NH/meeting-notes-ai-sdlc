# 06 Architecture and conventions

## Overview
```
Browser (React + Vite)  --HTTP/JSON + cookie-->  Express API  -->  SQLite file
        client/                                     server/          data/*.db
```

## Repository layout `[ASSUMPTION]`
```
client/            React app (Vite, Tailwind CSS, DaisyUI)
  src/api/         API calls (one module per resource)
  src/auth/        auth context, login/register hooks, protected route
  src/components/  reusable UI components
  src/pages/       one file per screen
  src/hooks/       custom hooks
server/
  src/routes/      HTTP layer only
  src/services/    business rules (BR-xx)
  src/repositories/ SQL only
  src/middleware/  authentication, validation, error handling
  migrations/      numbered .sql files
tests/             API and end-to-end automation tests
docs/              knowledge, stories, templates
```

## Layering rules
- **routes** parse and validate requests, call a service, and shape the response.
- **services** apply business rules and never touch HTTP objects.
- **repositories** contain all SQL (parameterized) and return plain objects.
  They always apply the soft-delete filter (BR-60, BR-61) so no other layer has to remember it.

## Authentication `[ASSUMPTION: mechanism]`
- Email + password. Passwords hashed with bcrypt (or argon2), never stored or logged in plain text (BR-52).
- On login the server sets a signed session token in an **httpOnly, SameSite=Lax cookie**
  (`Secure` in production). JavaScript in the browser cannot read it. Lifetime 8 hours (BR-55).
- An `authenticate` middleware runs before every `/api` route except `/auth/register`, `/auth/login`
  and the health check (BR-50). It rejects with `401` and puts the current user on the request.
- The frontend sends cookies with every request and redirects to Login on `401`.
- Login endpoint should have simple rate limiting to slow down password guessing.
- Frontend and API are served from the same origin (Vite dev proxy in development), so no CORS setup is needed.

## API outline (draft)
Base path `/api`. JSON only. Dates as ISO 8601 strings. Everything below requires login unless marked public.

| Method | Path | Purpose |
|--------|------|---------|
| POST | `/auth/register` | Create account and sign in (public, BR-51) |
| POST | `/auth/login` | Sign in (public, BR-53) |
| POST | `/auth/logout` | Sign out |
| GET | `/auth/me` | Current user, used on app start |
| GET | `/users` | List active users for attendee and owner pickers (no password data) |
| GET | `/meetings?search=` | List meetings, newest first, optional text search |
| POST | `/meetings` | Create meeting (with attendee user ids) |
| GET | `/meetings/:id` | Meeting with attendees, action items, decisions |
| PUT | `/meetings/:id` | Update meeting |
| DELETE | `/meetings/:id` | Soft delete meeting (BR-04, BR-60) |
| POST | `/meetings/:id/action-items` | Add action item |
| POST | `/meetings/:id/decisions` | Add decision |
| GET | `/action-items?status=&ownerId=&overdue=true` | Cross-meeting list for the dashboard |
| PATCH | `/action-items/:id` | Update fields or status |
| DELETE | `/action-items/:id` | Soft delete action item |
| PATCH | `/decisions/:id` | Edit decision |
| DELETE | `/decisions/:id` | Soft delete decision |
| GET | `/health` | Health check (public) |

## HTTP conventions
- Status codes: `200` OK, `201` created, `204` deleted, `400` validation error, `401` not logged in or bad credentials,
  `404` not found (including soft-deleted rows, BR-62), `409` conflict (for example email already registered), `500` unexpected.
- Error body: `{ "error": { "code": "VALIDATION_ERROR", "message": "..." } }`, optionally with `details` per field.
- List endpoints return `{ "data": [...] }`.
- A DELETE returns `204` and the row is flagged, not removed.

## UI conventions
- Use DaisyUI components (`table`, `badge`, `modal`, `btn`, `form-control`) before custom CSS.
- Status badges: Open = neutral, In progress = info, Done = success, Overdue = error.
- Destructive actions use a confirmation modal (BR-04).
- Protected routes redirect to Login when there is no session.

## Configuration
- Environment variables: `PORT`, `DATABASE_FILE`, `SESSION_SECRET` (required, long random value).
- Secrets are never committed. Provide a `.env.example` without real values.

## Non-functional targets `[ASSUMPTION]`
- List endpoints respond in under 500 ms for up to 10,000 action items.
- Works on current desktop and mobile browsers; keyboard accessible.
