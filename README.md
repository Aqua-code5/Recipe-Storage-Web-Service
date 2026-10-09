# Pantry frontend

A responsive, dependency-free frontend based on the supplied Pantry Figma design and recipe storage acceptance criteria. Includes collection search, author/category filtering, details, create/edit forms, delete confirmation, author/category creation, empty states, saving feedback, and recoverable API errors.

## Run

From this directory, run `python -m http.server 5173`, then open http://localhost:5173. Alternatively use any static HTTP server. Serve via HTTP rather than opening index.html directly because the JavaScript uses ES modules.

Demo mode starts with six recipes and saves changes in browser localStorage. It is a frontend demonstration, not MongoDB persistence or authentication. No backend was provided in this workspace.

## Connect the backend

Open **Connection settings** in the footer, select **REST API**, and set the base URL (for example `http://localhost:3000/api`). Enable CORS on Express for your frontend origin. API mode never falls back to demo data when a request fails.

`api.js` isolates data access. Expected JSON responses are direct objects/arrays or wrapped in `{ "data": ... }`. Errors should return `{ "message": "..." }`. IDs use `_id`. Recipe author/category fields may be IDs or populated objects. Recipe ingredients/instructions are arrays of strings (newline-separated strings are also accepted for display). Cooking time is `cookingTime`, an integer in minutes. Optional categories are sent as `null`.

| Method | Route | Purpose |
| --- | --- | --- |
| GET | /api/recipes | Collection |
| GET | /api/recipes/:id | Detail |
| POST | /api/recipes | Create |
| PUT | /api/recipes/:id | Edit |
| DELETE | /api/recipes/:id | Delete (200 or 204) |
| GET | /api/categories | Category options |
| POST | /api/categories | Create category |
| GET | /api/categories/:id/recipes | Category collection |
| GET | /api/users | Author options (additional endpoint needed by frontend) |
| POST | /api/users | Create author (additional endpoint needed by frontend) |
| GET | /api/users/:id/recipes | Author collection |

Recipe payload: `{ title, ingredients: string[], instructions: string[], cookingTime: number, author: userId, category: categoryId | null }`. User payload: `{ username, email }`. Category payload: `{ name }`.

The frontend uses native form validation plus whitespace and positive-integer checks. The backend still needs to validate data and references, implement Express/Mongoose models and routes, and connect to MongoDB Atlas. This frontend does not implement the PDF’s backend requirements.

## Files

- `index.html`: accessible application shell.
- `styles.css`: responsive Figma-inspired presentation.
- `app.js`: routing, forms, rendering, and interactions.
- `api.js`: REST adapter and explicitly selected persistent browser demo.

## Manual verification

Create a recipe, open its details, edit it, reload to check demo persistence, and delete it through confirmation. Add an author/category and select them in a new recipe. Check search and filters, an unmatched search, missing fields, and a mobile viewport. For backend integration, switch to API mode and verify each endpoint above, including invalid/unknown IDs and server failures.
