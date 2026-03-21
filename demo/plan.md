# Execution Plan

## Step 1: Project Setup + Server + Page (v0.1.0)

Everything in a single step — the project is small enough.

### 1.1 Project infrastructure
- `package.json` with scripts: `start`, `build`, `lint`, `test`
- `tsconfig.json` for TypeScript
- ESLint config
- Vitest config

### 1.2 Express server
- `src/server.ts` — Express app creation, static file serving, export for testing
- `src/index.ts` — entry point, starts listening on port 3000

### 1.3 Frontend
- `src/public/index.html` — page with "Click me" button, popup modal (hidden by default), "OK" button
- `src/public/style.css` — styling for button and popup
- `src/public/script.js` — click handlers for button and OK

### 1.4 Tests
- `test/server.test.ts` — HTTP-level tests (server responds, serves HTML, static files)
- `test/page.test.ts` — DOM-level tests (button exists, popup appears on click, OK closes popup, cycle repeats)

### Acceptance Criteria (Step 1)
- [ ] `npm run build` compiles
- [ ] `npm run lint` passes
- [ ] `npm test` all green
- [ ] Server serves the page on localhost:3000
- [ ] Button → popup → OK → dismiss cycle works
- [ ] Version in package.json: `0.1.0`
- [ ] CHANGELOG.md entry added
