# Step 1 — Coder Prompt: Server & Page Implementation

## ⚠️ Before starting
1. Read `./roles/coder.md` — this is your role
2. Read `spec.md` and `plan.md` for full context
3. Tests already exist in `test/` — your code must pass them. Do NOT modify test files.
4. When done: `git add src/ package.json tsconfig.json vitest.config.ts eslint.config.js && git commit -m "[coder] step1-1: express server with click-me page"`

## Context

We're building a minimal localhost website (REQ-1 through REQ-5 in spec.md):
- Express server on port 3000 serving static files
- HTML page with "Click me" button
- Popup modal with "Hellooo" text and "OK" button to dismiss
- Repeatable cycle

Tests are already written and will guide your implementation. Run `npm test` frequently.

## Tasks

### 1. Fix/update project infrastructure if needed

The tester set up `package.json`, `tsconfig.json`, `vitest.config.ts`, `eslint.config.js`. Review them and fix any issues. Ensure:
- `npm run build` compiles TypeScript
- `npm run lint` runs ESLint on `src/`
- `npm test` runs Vitest
- `npm start` runs the compiled server
- Version in `package.json` is `0.1.0`

### 2. Implement `src/server.ts`

Create the Express application:
- Create an Express app
- Serve static files from `src/public/` directory using `express.static`
- Export the `app` for testing (supertest needs it)
- Do NOT call `app.listen()` here — that goes in `index.ts`

### 3. Implement `src/index.ts`

Entry point:
- Import `app` from `./server.js`
- Call `app.listen(3000)` with a console.log confirming startup

### 4. Implement `src/public/index.html`

The HTML page must contain:
- A button with text **"Click me"** (use a `<button>` element)
- A popup/modal container (e.g., `<div id="popup">`) that is **hidden by default**
- Inside the popup: the text **"Hellooo"** and a button with text **"OK"**
- Link to `style.css` and `script.js`
- The popup should be hidden using inline style `display: none` or a CSS class — check what the tests expect

### 5. Implement `src/public/style.css`

Style the page:
- Center the "Click me" button on the page
- Style the popup as a modal overlay (centered, visible background overlay)
- Style the "OK" button inside the popup

### 6. Implement `src/public/script.js`

Frontend interaction logic:
- Click "Click me" → show the popup (set `display: block` or remove hidden class)
- Click "OK" → hide the popup (set `display: none` or add hidden class)
- Must be idempotent — multiple clicks don't break anything

## Acceptance Criteria

- [ ] `npm run build` — compiles without errors
- [ ] `npm run lint` — no errors
- [ ] `npm test` — ALL tests pass
- [ ] Server starts on port 3000 and serves the page
- [ ] Click me → Hellooo popup → OK → dismiss → repeatable

## Commit

When all checks pass:
```bash
git add src/ package.json tsconfig.json vitest.config.ts eslint.config.js && git commit -m "[coder] step1-1: express server with click-me page"
```
