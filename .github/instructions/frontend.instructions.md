---
applyTo: "client/**"
---

# Frontend rules (React, Vite, Tailwind CSS, DaisyUI)

- Use function components and hooks. One component per file, named in PascalCase.
- Style with Tailwind utility classes and **DaisyUI components** (`btn`, `card`, `modal`, `table`, ...)
  before writing custom CSS. Do not add another UI library.
- Keep components small. Move data fetching into a custom hook or a small API module
  (for example `client/src/api/meetings.js`), not inline inside components.
- Handle loading, empty and error states explicitly in every screen that loads data.
- Auth: keep the current user in one auth context (`client/src/auth/`). Wrap protected screens in a
  protected-route component that redirects to Login. Send requests with cookies (`credentials: 'include'`),
  and redirect to Login when the API returns `401`. Never store tokens or passwords in `localStorage`.
- "Delete" buttons always open a confirmation modal.
- Keep state as local as possible. Do not introduce a state management library unless `plan.md` justifies it.
- Accessibility: use semantic elements, labels on form fields, and keyboard-reachable controls.
- Tests: Vitest + React Testing Library. Test what the user sees and does, not implementation details.
