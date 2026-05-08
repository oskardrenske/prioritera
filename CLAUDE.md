# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the app

No build step. Open `index.html` directly in a browser, or serve it with any static file server:

```bash
npx serve .
# or
python3 -m http.server
```

## Architecture

The entire application is a single self-contained `index.html` with inline CSS and JavaScript — no framework, no dependencies, no bundler.

**Data model** (in-memory only, resets on page reload):

- `objects` — array of `{ id, name, color }` items, initialized with random JIRA-style IDs
- `prioStack` — ordered array of item IDs representing the Todo stack (only the top item is interactive)
- `backlog` — ordered array of item IDs representing the Backlog shelf (all items are clickable)

**Rendering** is fully imperative: `render()` wipes both DOM containers and rebuilds them from scratch on every state change. `createObjectEl()` attaches click (move item) and dblclick (inline rename via `contentEditable`) handlers.

**Interaction rules:**
- Clicking a backlog item moves it to the top of the Todo stack
- Only the topmost Todo item can be clicked back to backlog
- Double-clicking any item's label makes it inline-editable; Enter or blur commits the change