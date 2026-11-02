---
applyTo: "tests/**"
---

# Automation test rules (API with Supertest, end-to-end with Playwright)

These rules apply to every file under `tests/`, whichever agent writes it.
Unit tests that sit next to the code follow the frontend and backend instruction files instead.

## Structure
- `tests/api/` for API tests, `tests/e2e/` for browser tests, `tests/helpers/` for shared builders and login helpers.
- One test file per feature or resource (for example `meetings.api.test.js`).
- Test names describe behaviour: `should_<expected result>_when_<condition>`.

## Traceability
- Every test states what it covers in a comment or in its title, for example `// Covers AC-2, TC-5, BR-12`.
- Keep the traceability table in `docs/stories/<STORY-ID>/test-cases.md` in sync (AC -> test case -> automated test).

## Independence and stability
- Each test creates its own data and does not depend on test order or on data left by another test.
- API tests run against an **in-memory SQLite database**, created fresh for each test file and cleaned between tests.
- Never depend on today's date. Use fixed dates in the test data, or fake the clock, so "overdue" tests do not break tomorrow.
- No real network calls and no `sleep` or fixed waits. Wait for a condition (Playwright auto-waiting and `expect`).
- A flaky test is fixed or quarantined with a reason. Do not hide it behind blind retries.
- Never weaken or delete an assertion to make a test pass. If the product is wrong, report a bug.

## Authentication in tests
- Use a shared helper that registers a user and logs in, and returns the session cookie (API) or the stored login state (Playwright `storageState`).
- Do not log in through the UI in every e2e test. Log in through the UI only in the tests that cover login itself.
- Every protected endpoint has at least one test for `401` without a session (BR-50).

## Soft delete in tests
- After a delete, assert the row is hidden from lists, search and the dashboard, and returns `404` by id (BR-60 to BR-62).
- Where useful, assert the row still exists in the database with `is_deleted = 1`.

## API tests (Supertest)
- Assert the status code, the response shape and the error `code`, not only the message text.
- Cover the happy path, validation errors (BR-01, BR-10, BR-41), not found, and conflicts.
- Build test data with small builder functions (for example `buildMeeting({ title: 'Weekly sync' })`), not long inline objects.

## End-to-end tests (Playwright)
- Select elements by role, label or visible text (`getByRole`, `getByLabel`, `getByText`). Do not use Tailwind or DaisyUI class names.
- Set up data through the API where the setup itself is not the thing under test.
- Test user-visible behaviour of a whole flow (see `docs/knowledge/04-user-roles-and-flows.md`), not internal state.
- Check empty, loading and error states for list screens.

## Readability
- One behaviour per test. Arrange, act, assert, in that order, separated by blank lines.
- Prefer clear and repetitive over clever and shared. Do not hide assertions inside helpers.
