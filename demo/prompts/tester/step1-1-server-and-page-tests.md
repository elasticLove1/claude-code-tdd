# Step 1 — Tester Prompt: Server & Page Tests

## ⚠️ Before starting
1. Read `./roles/tester.md` — this is your role
2. Read `spec.md` and `plan.md` for full context
3. When done: `git add test/ && git commit -m "[tester] step1-1: server and page tests"`

## Context

We're building a minimal localhost website (REQ-1 through REQ-5 in spec.md). An Express server serves a static HTML page with a "Click me" button. Clicking it shows a popup with "Hellooo" and an "OK" button that dismisses it.

There is NO code yet — tests will fail, and that's expected (TDD).

## Project setup (you must do this first)

Before writing tests, initialize the project so `npm test` can run:

```bash
npm init -y
npm install --save express
npm install --save-dev typescript vitest supertest jsdom @types/express @types/supertest @types/node eslint @eslint/js @typescript-eslint/eslint-plugin @typescript-eslint/parser
```

Create `tsconfig.json`:
```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "ES2020",
    "moduleResolution": "bundler",
    "outDir": "dist",
    "rootDir": ".",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "declaration": true,
    "sourceMap": true,
    "resolveJsonModule": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist", "test"]
}
```

Create `vitest.config.ts`:
```ts
import { defineConfig } from 'vitest/config';

export default defineConfig({
  test: {
    globals: true,
    environment: 'node',
    include: ['test/**/*.test.ts'],
    testTimeout: 10000,
  },
});
```

Create `eslint.config.js`:
```js
import tseslint from '@typescript-eslint/eslint-plugin';
import tsparser from '@typescript-eslint/parser';

export default [
  {
    files: ['src/**/*.ts'],
    languageOptions: {
      parser: tsparser,
      parserOptions: {
        ecmaVersion: 2020,
        sourceType: 'module',
      },
    },
    plugins: {
      '@typescript-eslint': tseslint,
    },
    rules: {
      ...tseslint.configs.recommended.rules,
    },
  },
];
```

Update `package.json` scripts:
```json
{
  "type": "module",
  "version": "0.1.0",
  "scripts": {
    "start": "node dist/src/index.js",
    "build": "tsc",
    "lint": "eslint src/",
    "test": "vitest run"
  }
}
```

Create placeholder source files so TypeScript imports resolve (these are empty stubs, NOT implementation):

`src/server.ts`:
```ts
// Placeholder — will be implemented by coder
import express from 'express';
const app = express();
export { app };
```

`src/index.ts`:
```ts
// Placeholder — will be implemented by coder
```

Create directories: `mkdir -p src/public`

Create placeholder `src/public/index.html`:
```html
<!-- Placeholder — will be implemented by coder -->
<!DOCTYPE html><html><head></head><body></body></html>
```

## Test File 1: `test/server.test.ts` — HTTP-level tests

Use `supertest` to test the Express app. Import `app` from `../src/server.ts`.

**Supertest API reference:**
```ts
import request from 'supertest';
import { app } from '../src/server.js';

const response = await request(app).get('/');
// response.status — HTTP status code (number)
// response.text — response body as string
// response.headers['content-type'] — content-type header
```

### Scenarios:

| # | Scenario | Input | Expected |
|---|----------|-------|----------|
| 1 | GET / returns 200 | `GET /` | `status === 200` |
| 2 | GET / returns HTML | `GET /` | `content-type` contains `text/html` |
| 3 | HTML contains "Click me" button | `GET /` | `response.text` includes a button with text "Click me" |
| 4 | HTML contains popup structure | `GET /` | `response.text` includes element with "Hellooo" text and "OK" button |
| 5 | Popup is hidden by default | `GET /` | Popup container has a hidden/display-none style or class |
| 6 | Static CSS is served | `GET /style.css` | `status === 200`, `content-type` contains `text/css` |
| 7 | Static JS is served | `GET /script.js` | `status === 200`, `content-type` contains `javascript` |
| 8 | 404 for unknown routes | `GET /nonexistent` | `status === 404` |

## Test File 2: `test/page.test.ts` — DOM-level tests

Use `jsdom` to test frontend behavior. Load the HTML from the server response or from file, then execute the script.

**jsdom API reference:**
```ts
import { JSDOM } from 'jsdom';
import fs from 'fs';
import path from 'path';

// Load HTML
const html = fs.readFileSync(path.join(__dirname, '../src/public/index.html'), 'utf-8');
const dom = new JSDOM(html, { runScripts: 'dangerously', resources: 'usable' });
const { document } = dom.window;

// Or inline for more control:
const dom = new JSDOM(html, {
  runScripts: 'dangerously',
  url: 'http://localhost:3000',
});

// Load script manually if needed:
const scriptContent = fs.readFileSync(path.join(__dirname, '../src/public/script.js'), 'utf-8');
dom.window.eval(scriptContent);

// Query elements
const button = document.querySelector('button');
// Dispatch events
button?.click(); // or button?.dispatchEvent(new dom.window.Event('click'));
// Check visibility
const popup = document.getElementById('popup');
// popup.style.display, popup.classList, popup.hidden, etc.
```

### Scenarios:

| # | Scenario | Action | Expected |
|---|----------|--------|----------|
| 1 | Page has "Click me" button | Load page | `document.querySelector` finds a button with text "Click me" |
| 2 | Popup is hidden initially | Load page | Popup element exists but is not visible (hidden attribute, display:none, or CSS class) |
| 3 | Clicking "Click me" shows popup | Click the button | Popup becomes visible |
| 4 | Popup contains "Hellooo" | Click the button | Popup text content includes "Hellooo" |
| 5 | Popup has "OK" button | Click the button | Popup contains a button with text "OK" |
| 6 | Clicking "OK" hides popup | Click "Click me", then click "OK" | Popup is hidden again |
| 7 | Cycle is repeatable | Click "Click me" → "OK" → "Click me" again | Popup shows again on second click |
| 8 | "Click me" button remains after dismiss | Full cycle | "Click me" button is still present and functional |

## Boundary/Edge cases to include:

- Multiple rapid clicks on "Click me" — popup should still work correctly
- Click "OK" when popup is already hidden — no errors thrown

## Commit

When done:
```bash
git add -A && git commit -m "[tester] step1-1: server HTTP tests and page DOM tests"
```
