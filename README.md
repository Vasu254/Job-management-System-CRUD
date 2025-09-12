# PFM — Personal Finance Manager

A small full-stack Personal Finance Manager (PFM) project with a React + Vite frontend and a Node.js + Express backend (PostgreSQL). This repository contains a simple CRUD app for clients/transactions and can be extended with authentication (Clerk, Google OAuth), JWT-protected APIs, and analytics (student analysis / spending insights).

## Contents

- `Frontend/` — Vite + React application (UI, forms, charts). Uses Tailwind + daisyUI for styling.
- `Backend/` — Node.js + Express API, connects to PostgreSQL. Holds routes, controllers, and DB config.

## Tech stack

- Frontend: React, Vite, Tailwind CSS, daisyUI
- Backend: Node.js, Express
- Database: PostgreSQL
- Auth (recommended): Clerk (frontend-first) or Passport + Google OAuth (backend)

## Prerequisites

- Node.js (v16+ recommended)
- npm or yarn
- PostgreSQL running locally or remotely
- Optional: Clerk account (for quick auth) and Google Cloud credentials (if using Google OAuth directly)

## Environment variables

Create `.env` files for both `Backend/` and `Frontend/` (do not commit these files).

Backend `.env` (example — update with your values):

```
# Database
PG_USER=postgres
PG_HOST=localhost
PG_DATABASE=client_db
PG_PASSWORD=your_db_password
PG_PORT=5432

# Auth (optional, for Google OAuth or JWT)
GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret
JWT_SECRET=your_jwt_secret

# Server
PORT=3000
NODE_ENV=development
```

Frontend `.env` (example):

```
VITE_CLERK_PUBLISHABLE_KEY=your_clerk_publishable_key
VITE_GOOGLE_CLIENT_ID=your_google_client_id (if using client-side Google flows)
```

## Database setup

1. Create a PostgreSQL database matching `PG_DATABASE`.
2. Run the project's migrations or create tables manually. Minimal tables required:

```sql
CREATE TABLE users (
	id SERIAL PRIMARY KEY,
	email TEXT UNIQUE,
	password_hash TEXT,
	google_id TEXT,
	created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE transactions (
	id SERIAL PRIMARY KEY,
	user_id INTEGER REFERENCES users(id),
	category TEXT,
	amount NUMERIC,
	note TEXT,
	created_at TIMESTAMP DEFAULT NOW()
);
```

Adjust schema to your needs.

## How to run (development)

Open two terminals.

1. Backend

```fish
cd Backend
npm install
# ensure .env is present
npm run dev
```

Default server runs on `http://localhost:3000` (adjust `PORT` in `.env`).

2. Frontend

```fish
cd Frontend
npm install
# ensure Frontend/.env is present (Clerk key or Google client id)
npm run dev
```

Frontend runs on `http://localhost:5173` by default.

## Auth options and notes

- Clerk (recommended): Frontend-first, fastest integration. Create a Clerk app, set `VITE_CLERK_PUBLISHABLE_KEY`. Wrap your app with `ClerkProvider`, add `SignIn`/`SignUp` pages, and protect routes with Clerk hooks.
- Google OAuth via Passport (backend): Use `passport-google-oauth20` and exchange Google profile for a user record, then issue a JWT.
- JWT: Backend should issue JWT on login/register; frontend stores token (e.g., localStorage) and sends `Authorization: Bearer <token>` on protected API calls.

## Student analysis / Analytics

- Implement aggregation queries in the backend (e.g., SUM(amount) GROUP BY category, monthly trends) and expose `/api/analysis` endpoints.
- Frontend can visualize results using Chart.js, Recharts, or similar.

## CORS

If frontend and backend run on different ports in dev, enable CORS in the backend to allow `http://localhost:5173`.

## Deployment

- Frontend: Vercel / Netlify (set environment variables in the dashboard).
- Backend: Heroku / Render / Railway / DigitalOcean. Ensure `NODE_ENV=production`, set `DATABASE_URL`, and enable HTTPS for OAuth redirects.

## Useful commands

- Lint / format (if configured): `npm run lint`, `npm run format`
- Run tests (if present): `npm test`

## Next steps / recommended improvements

- Add Clerk or Passport-based authentication (Clerk is faster).
- Add backend input validation and rate limiting.
- Add unit/integration tests for auth and analysis endpoints.
- Add migrations with a tool like Sequelize, Knex, or TypeORM.

---

If you want, I can: add Clerk integration code to `Frontend/src/App.jsx`, create `SignIn/SignUp` pages, scaffold backend auth routes/controllers, or add analysis endpoints. Tell me which piece to implement first.
