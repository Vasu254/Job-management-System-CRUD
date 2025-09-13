# Client Management System

A full-stack CRUD application for managing client details, including salary, job status (active/inactive), and other information. Built with React + Vite frontend and Node.js + Express backend, storing data in PostgreSQL.

## Contents

- `Frontend/` — Vite + React application (UI, forms, tables). Uses Tailwind + daisyUI for styling.
- `Backend/` — Node.js + Express API, connects to PostgreSQL. Holds routes, controllers, and DB config.

## Tech Stack

- Frontend: React, Vite, Tailwind CSS, daisyUI
- Backend: Node.js, Express
- Database: PostgreSQL

## Prerequisites

- Node.js (v16+ recommended)
- npm or yarn
- PostgreSQL running locally or remotely

## Environment Variables

Create `.env` files for both `Backend/` and `Frontend/` (do not commit these files).

Backend `.env` (example — update with your values):

```
# Database
PG_USER=postgres
PG_HOST=localhost
PG_DATABASE=client_db
PG_PASSWORD=your_db_password
PG_PORT=5432

# Server
PORT=3000
NODE_ENV=development
```

Frontend `.env` (example — if needed for any API keys):

```
# Add any frontend-specific environment variables here
```

## Database Setup

1. Create a PostgreSQL database matching `PG_DATABASE`.
2. Create the clients table manually or via migrations:

```sql
CREATE TABLE clients (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL,
    salary NUMERIC,
    job_status TEXT CHECK (job_status IN ('active', 'inactive')),
    email TEXT,
    phone TEXT,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);
```

Adjust schema to your needs.

## How to Run (Development)

Open two terminals.

1. Backend

```fish
cd Backend
npm install
# ensure .env is present
nodemon src/index.js
```

Default server runs on `http://localhost:3000` (adjust `PORT` in `.env`).

2. Frontend

```fish
cd Frontend
npm install
npm run dev
```

Frontend runs on `http://localhost:5173` by default.

## CORS

If frontend and backend run on different ports in dev, enable CORS in the backend to allow `http://localhost:5173`.

## Deployment

- Frontend: Vercel / Netlify (set environment variables in the dashboard).
- Backend: Heroku / Render / Railway / DigitalOcean. Ensure `NODE_ENV=production`, set `DATABASE_URL`.

## Useful Commands

- Lint / format (if configured): `npm run lint`, `npm run format`
- Run tests (if present): `npm test`

## Next Steps / Recommended Improvements

- Add input validation and error handling.
- Add unit/integration tests for CRUD endpoints.
- Implement pagination and search for client list.
- Add migrations with a tool like Sequelize, Knex, or TypeORM.

---

This project provides basic CRUD operations for client management. If you need help implementing specific features, let me know.

Repository: [git@github.com:Vasu254/PFM.git](git@github.com:Vasu254/PFM.git)
