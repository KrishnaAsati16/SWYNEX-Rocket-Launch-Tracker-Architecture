# Rocket Launch Tracker

## About the project

Rocket Launch Tracker is a small full-stack web application for people who follow space launches. It is being built as part of my Full Stack Developer Internship at SWYNEX Technologies. A user can browse upcoming and past launches, save the ones they care about to a personal watchlist, and write notes on each saved launch.

The project is built step by step. The architecture and the backend (API and database) are done. The frontend is the next part.

## Main features

| Feature | Description |
|---------|-------------|
| Authentication | Register and log in with email and password (JWT) |
| Launch list | Browse launches, search by name, filter by status |
| Launch detail | See rocket, agency, site, date, status and description |
| Watchlist | Add or remove a launch from my watchlist |
| Notes | Write and edit a personal note on a watchlisted launch |
| Admin launch management | An admin can add, edit and delete launches |

## Technology stack

| Part | Technology |
|------|------------|
| Frontend | React.js with Vite (planned) |
| Backend | Node.js and Express.js |
| Database | PostgreSQL |
| Validation | Zod |
| Authentication | JWT and bcrypt |
| API | REST API |
| Version control | Git and GitHub |

## How the app works

1. A visitor opens the website and browses the list of launches.
2. They can search by name or filter by status (upcoming, success, failed, delayed).
3. They open a launch to see its full details.
4. After registering or logging in, they add launches to their watchlist.
5. On the My Watchlist page they write or edit notes and remove launches.

## Project folders

| Folder / File | Purpose |
|---------------|---------|
| `backend/` | Node.js and Express backend with the PostgreSQL database layer |
| `backend/src/` | Routes, controllers, middleware and validation |
| `backend/sql/schema.sql` | Table definitions |
| `backend/tests.http` | Ready-made API requests for testing |
| `frontend/` | React frontend (to be added) |
| `docs/` | Project documents |
| `docs/architecture.md` | Detailed project architecture |
| `README.md` | Project introduction and setup information |

## Database tables

| Table | Information stored |
|-------|--------------------|
| users | Name, email, password hash and role |
| launches | Name, rocket, agency, launch site, date, status and description |
| watchlist_items | User, launch and personal note |

A user can save many launches, and a launch can be saved by many users. The `watchlist_items` table connects the two, and a user cannot add the same launch twice.

## API overview

Protected routes need the header `Authorization: Bearer <token>`. More details and examples are in [backend/README.md](backend/README.md).

### Auth
| Method | Route | Access | Purpose |
|--------|-------|--------|---------|
| POST | `/api/auth/register` | Public | Register a user |
| POST | `/api/auth/login` | Public | Log in and get a token |
| GET | `/api/auth/me` | Logged in | Current user's profile |

### Launches
| Method | Route | Access | Purpose |
|--------|-------|--------|---------|
| GET | `/api/launches` | Public | List launches (`?search=` and `?status=`) |
| GET | `/api/launches/:id` | Public | Show one launch |
| POST | `/api/launches` | Admin | Add a launch |
| PATCH | `/api/launches/:id` | Admin | Edit a launch |
| DELETE | `/api/launches/:id` | Admin | Delete a launch |

### Watchlist
| Method | Route | Access | Purpose |
|--------|-------|--------|---------|
| GET | `/api/watchlist` | Logged in | View my watchlist |
| POST | `/api/watchlist` | Logged in | Add a launch to my watchlist |
| PATCH | `/api/watchlist/:id` | Logged in | Update the note on a saved launch |
| DELETE | `/api/watchlist/:id` | Logged in | Remove a launch from my watchlist |

## Validation and errors

Every request body, query and id is validated with Zod. Invalid input returns status `400` with a list of the problems:

```json
{
  "error": "Validation failed",
  "details": [
    { "field": "name", "message": "Name must be at least 2 characters" }
  ]
}
```

Other status codes: `401` not logged in or bad token, `403` not an admin, `404` not found, `409` duplicate (same email, or launch already in the watchlist).

## Frontend screens (planned)

| Screen | Route |
|--------|-------|
| Register | `/register` |
| Login | `/login` |
| Launch List | `/` |
| Launch Detail | `/launches/:id` |
| My Watchlist | `/watchlist` |
| Profile (optional) | `/profile` |

## Setup (backend)

You need Node.js 18 or newer and PostgreSQL.

1. Create a PostgreSQL database named `rocket_launch_tracker`.
2. Go to the backend folder and install packages:
   ```
   cd backend
   npm install
   ```
3. Copy `.env.example` to `.env` and fill in your database password and a long random `JWT_SECRET`.
4. Create the tables and add sample data:
   ```
   npm run db:init
   npm run db:seed
   ```
5. Start the server:
   ```
   npm run dev
   ```
   The API runs at `http://localhost:5000`. Open `http://localhost:5000/api/health` to check it.

Sample accounts from the seed (for local testing only):

| Role | Email | Password |
|------|-------|----------|
| Admin | admin@example.com | Admin@12345 |
| User | user@example.com | User@12345 |

To try the API, open `backend/tests.http` in VS Code with the REST Client extension, or copy the requests into Postman.

## Future improvements

After the frontend is done, I would like to add pagination on the launch list, an admin screen for managing launches, and importing launch data from a public API.

## Current project status

| Work | Status |
|------|--------|
| Project idea | Completed |
| Architecture document | Completed |
| Starter repository | Completed |
| Backend development | Completed |
| Database setup | Completed |
| API testing | Completed (manual tests with `tests.http`) |
| Frontend development | Not started |

This repository now contains the architecture documentation and the working backend with its database. The frontend will be built next.

## Author

Krishna Asati, Full Stack Developer Intern at SWYNEX Technologies.
