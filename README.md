# Rocket Launch Tracker

## About the project

Rocket Launch Tracker is a small full-stack web application for people who follow space launches. It is being built as part of my Full Stack Developer Internship task at SWYNEX Technologies. A user can browse upcoming and past launches, save the ones they care about to a personal watchlist, and write notes on each saved launch.

I am starting with the basic features first. Once the main application is working properly, more features can be added later.

## Main features

| Feature | Description |
|---------|-------------|
| Authentication | Register and log in with email and password |
| Launch list | Browse launches, search by name, filter by status |
| Launch detail | See rocket, agency, site, date, status and description |
| Watchlist | Add or remove a launch from my watchlist |
| Notes | Write and edit a personal note on a watchlisted launch |
| Admin launch management | An admin can add launches (basic version) |

## Technology stack

| Part | Technology |
|------|------------|
| Frontend | React.js with Vite |
| Backend | Node.js and Express.js |
| Database | PostgreSQL |
| Authentication | JWT and bcrypt |
| API | REST API |
| Version control | Git and GitHub |

## How the app will work

1. A visitor opens the website and browses the list of launches.
2. They can search by name or filter by status (upcoming, success, failed, delayed).
3. They open a launch to see its full details.
4. After registering or logging in, they add launches to their watchlist.
5. On the My Watchlist page they write or edit notes and remove launches.

## Project folders

| Folder / File | Purpose |
|---------------|---------|
| `frontend/` | React frontend |
| `backend/` | Node.js and Express backend |
| `docs/` | Project documents |
| `docs/architecture.md` | Detailed project architecture |
| `README.md` | Project introduction and setup information |

## Database tables

| Table | Information stored |
|-------|--------------------|
| User | Name, email, password hash and role |
| Launch | Name, rocket, agency, launch site, date, status and description |
| WatchlistItem | User, launch and personal note |

A user can save many launches, and a launch can be saved by many users. The WatchlistItem table connects the two.

## API overview

The frontend and backend communicate using REST APIs. These are the main routes planned for the first version. The full list is in [docs/architecture.md](docs/architecture.md).

| Method | Route | Purpose |
|--------|-------|---------|
| POST | `/api/auth/register` | Register a user |
| POST | `/api/auth/login` | Log in |
| GET | `/api/launches` | Show launches (search and status filter) |
| GET | `/api/launches/:id` | Show one launch |
| GET | `/api/watchlist` | View my watchlist |
| POST | `/api/watchlist` | Add a launch to my watchlist |
| PATCH | `/api/watchlist/:id` | Update the note on a saved launch |
| DELETE | `/api/watchlist/:id` | Remove a launch from my watchlist |

## Frontend screens

| Screen | Route |
|--------|-------|
| Register | `/register` |
| Login | `/login` |
| Launch List | `/` |
| Launch Detail | `/launches/:id` |
| My Watchlist | `/watchlist` |
| Profile (optional) | `/profile` |

## Future improvements

After completing the basic version, I would like to add pagination on the launch list, an admin screen for managing launches, and importing launch data from a public API.

## Setup

Setup steps will be added here once the backend and frontend are built.

## Current project status

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

## Author

Krishna Asati, Full Stack Developer Intern at SWYNEX Technologies.
