# Rocket Launch Tracker

**Author:** Krishna Asati
**Internship:** Full Stack Developer Internship, SWYNEX Technologies
**Date:** November 2026

---

## 1. Overview

Rocket Launch Tracker is a small full-stack web application for people who follow space launches. A user can browse upcoming and past launches, save the ones they care about to a personal watchlist, and write notes on each saved launch.

I picked this idea for two reasons. First, I am interested in aerospace, so I will actually enjoy building it. Second, the scope is small enough to finish in the 30-day internship while still covering the things a full-stack app needs: authentication, a relational data model, a REST API, and a multi-page frontend.

### Goals
- Let users register, log in and keep their own data private.
- Show launches clearly, with search and status filtering.
- Let each user manage a personal watchlist with notes.
- Keep the design simple enough to build the backend in Task 2 and connect the frontend in Task 3 without redesigning anything.

### Out of scope (for now)
- Live telemetry or real-time updates
- Payments, notifications or email reminders
- Pulling data from third-party launch APIs (I will seed the database manually at first)

---

## 2. Features

| # | Feature | Description |
|---|---------|-------------|
| 1 | Authentication | Register and log in with email and password |
| 2 | Launch list | Browse launches, search by name, filter by status |
| 3 | Launch detail | See rocket, agency, site, date, status and description |
| 4 | Watchlist | Add or remove a launch from my watchlist |
| 5 | Notes | Write and edit a personal note on a watchlisted launch |
| 6 | Admin launch management | An admin can add launches (basic version) |

---

## 3. Tech Stack

| Layer | Choice | Why |
|-------|--------|-----|
| Frontend | React (Vite) | Component-based, fast setup, I am comfortable with it |
| Backend | Node.js with Express | Simple REST API, same language as the frontend |
| Database | PostgreSQL | The data is relational (users, launches, watchlist links) |
| Auth | JWT with bcrypt password hashing | Stateless and easy to use from a React client |
| Version control | Git and GitHub | Required for submission |

---

## 4. Data Models

### 4.1 User
| Field | Type | Notes |
|-------|------|-------|
| id | integer | Primary key |
| name | string | Required |
| email | string | Required, unique |
| passwordHash | string | bcrypt hash, the plain password is never stored |
| role | enum | `user` or `admin`, default `user` |
| createdAt | timestamp | Set automatically |

### 4.2 Launch
| Field | Type | Notes |
|-------|------|-------|
| id | integer | Primary key |
| name | string | Mission name |
| rocket | string | Vehicle name |
| agency | string | Organisation running the launch |
| launchSite | string | Location of the launch |
| launchDate | timestamp | Planned or actual launch time |
| status | enum | `upcoming`, `success`, `failed`, `delayed` |
| description | text | Short summary of the mission |

### 4.3 WatchlistItem
| Field | Type | Notes |
|-------|------|-------|
| id | integer | Primary key |
| userId | integer | Foreign key to User |
| launchId | integer | Foreign key to Launch |
| note | text | Optional personal note |
| createdAt | timestamp | Set automatically |

A unique constraint on (`userId`, `launchId`) prevents the same launch from being added twice by the same user.

### 4.4 Relationships
- One User has many WatchlistItems.
- One Launch can appear in many users' watchlists.
- User and Launch are therefore many-to-many, and WatchlistItem is the join table that also carries the note.

```
User 1 ────< WatchlistItem >──── 1 Launch
```

---

## 5. API Design

Base path: `/api`. Protected routes expect the header `Authorization: Bearer <token>`.

### 5.1 Auth
| Method | Route | Auth | Purpose |
|--------|-------|------|---------|
| POST | /api/auth/register | No | Create an account |
| POST | /api/auth/login | No | Log in and receive a JWT |
| GET | /api/auth/me | Yes | Get the current user's profile |

Example login request and response:
```json
// POST /api/auth/login
{ "email": "user@example.com", "password": "secret123" }

// 200 OK
{ "token": "<jwt>", "user": { "id": 1, "name": "Krishna", "role": "user" } }
```

### 5.2 Launches
| Method | Route | Auth | Purpose |
|--------|-------|------|---------|
| GET | /api/launches | No | List launches. Supports `?search=` and `?status=` |
| GET | /api/launches/:id | No | Get one launch |
| POST | /api/launches | Admin | Add a launch |
| PATCH | /api/launches/:id | Admin | Edit a launch |

### 5.3 Watchlist
| Method | Route | Auth | Purpose |
|--------|-------|------|---------|
| GET | /api/watchlist | Yes | List my watchlist with launch details |
| POST | /api/watchlist | Yes | Add a launch (`{ "launchId": 3 }`) |
| PATCH | /api/watchlist/:id | Yes | Update the note on an item |
| DELETE | /api/watchlist/:id | Yes | Remove an item |

### 5.4 Error handling
The API returns standard status codes: `400` for invalid input, `401` for missing or invalid token, `403` for insufficient role, `404` for missing records, `409` for duplicates (for example, adding the same launch twice). Error bodies use one shape: `{ "error": "message" }`.

---

## 6. Frontend Screens

| Screen | Route | Purpose |
|--------|-------|---------|
| Register | /register | Create an account |
| Login | /login | Sign in |
| Launch List | / | Browse launches with search and status filter |
| Launch Detail | /launches/:id | Full details and an "Add to watchlist" button |
| My Watchlist | /watchlist | Saved launches with editable notes |
| Profile | /profile | Basic account info (optional) |

### User flow
1. A new visitor lands on the Launch List and can browse without an account.
2. To save a launch, they register or log in.
3. After login, they add launches to the watchlist from the list or detail page.
4. On My Watchlist they write or edit notes and remove launches they no longer care about.

### Frontend structure
- `pages/` holds one component per screen.
- `components/` holds reusable pieces such as LaunchCard, SearchBar and Navbar.
- `api/` holds the functions that call the backend, so the screens never build requests by hand.
- The auth token is kept on the client and attached to every protected request. Protected pages redirect to Login when there is no valid token.

---

## 7. Security Notes

- Passwords are hashed with bcrypt before storage.
- Protected routes verify the JWT in a middleware.
- Watchlist routes always filter by the logged-in user's id, so one user cannot read or edit another user's data.
- Secrets such as the database URL and JWT secret live in a `.env` file, which is not committed. A `.env.example` documents the required variables.

---

## 8. Repository Structure

```
SWYNEX-Project-Architecture/
├── README.md
├── docs/
│   └── architecture.md
├── backend/
│   ├── src/
│   │   ├── routes/
│   │   ├── controllers/
│   │   ├── models/
│   │   └── middleware/
│   ├── package.json
│   └── .env.example
└── frontend/
    ├── src/
    │   ├── pages/
    │   ├── components/
    │   └── api/
    └── package.json
```

---

## 9. Current Project Status

| Work | Status |
|------|--------|
| Project idea | Planned |
| Architecture document | Prepared |
| Starter repository | Prepared |
| Backend development | Not started |
| Database setup | Not started |
| Frontend development | Not started |
| Testing | Not started |

This repository currently contains the project plan and documentation. The actual application will be developed step by step.
