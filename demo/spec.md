# Marshall — Click Me Website

## Overview

A minimal localhost website with a single interactive element: a "Click me" button that triggers a popup with the message "Hellooo" and an "OK" button to dismiss it.

## Requirements

### REQ-1: Web Server
The application runs an HTTP server on `localhost` (default port 3000) that serves the website.

### REQ-2: Click Me Button
The page displays a single button labeled **"Click me"**.

### REQ-3: Popup on Click
When the "Click me" button is pressed, a popup/modal appears containing:
- The text **"Hellooo"**
- An **"OK"** button

### REQ-4: Dismiss Popup
When the "OK" button is pressed, the popup closes and the page returns to its initial state (the "Click me" button is visible and functional again).

### REQ-5: Repeatability
The interaction is repeatable — after dismissing the popup, pressing "Click me" again shows the popup again.

## Tech Stack

- **Runtime**: Node.js + TypeScript
- **Server**: Express.js
- **Frontend**: Static HTML + vanilla CSS/JS (served by Express)
- **Testing**: Vitest (unit/integration), Supertest (HTTP), jsdom (DOM)
- **Linting**: ESLint
- **Build**: TypeScript compiler (`tsc`)

## Acceptance Criteria

1. `npm start` launches the server on `http://localhost:3000`
2. Navigating to `http://localhost:3000` shows a page with a "Click me" button
3. Clicking the button shows a popup with "Hellooo" and an "OK" button
4. Clicking "OK" closes the popup
5. The cycle can be repeated indefinitely
6. `npm test` — all tests green
7. `npm run lint` — no errors
8. `npm run build` — compiles without errors
