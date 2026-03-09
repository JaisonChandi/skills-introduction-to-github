# 🔔 Subscription Detection System

A full-stack web application that helps users track and manage all their ongoing app and service subscriptions in one place.

## Tech Stack

| Layer     | Technology                     |
|-----------|-------------------------------|
| Frontend  | React 18                      |
| Backend   | Node.js + Express             |
| Database  | PostgreSQL                    |

## Features

- **Dashboard** – View all subscriptions with at-a-glance statistics (total count, active count, monthly & annual spend).
- **Add / Edit / Delete** subscriptions via a modal form.
- **Renewal alerts** – Cards highlight subscriptions renewing within 7 days.
- **Filter** by status: All, Active, Paused, Cancelled.
- **Category icons** for quick visual identification (Streaming, Music, Gaming, Productivity, etc.).
- Billing-cycle aware cost calculation (Monthly / Quarterly / Yearly).

## Project Structure

```
.
├── backend/
│   ├── db/
│   │   └── schema.sql          # PostgreSQL schema
│   ├── routes/
│   │   └── subscriptions.js    # CRUD REST routes
│   ├── db.js                   # PostgreSQL connection pool
│   ├── server.js               # Express entry point
│   ├── .env.example            # Environment variable template
│   └── package.json
└── frontend/
    ├── public/
    │   └── index.html
    ├── src/
    │   ├── components/
    │   │   ├── SubscriptionCard.jsx
    │   │   ├── SubscriptionForm.jsx
    │   │   └── SubscriptionList.jsx
    │   ├── services/
    │   │   └── api.js          # Fetch-based API client
    │   ├── App.jsx
    │   ├── index.css
    │   └── index.js
    ├── .env.example
    └── package.json
```

## Getting Started

### Prerequisites

- Node.js >= 18
- PostgreSQL >= 14

### 1. Database Setup

```bash
# Create the database
createdb subscription_db

# Apply the schema
psql -d subscription_db -f backend/db/schema.sql
```

### 1A. Database Setup with Docker Compose (Recommended)

```bash
# Start PostgreSQL in Docker
docker compose up -d db

# Stop PostgreSQL
docker compose stop db
```

Notes:
- The Compose setup uses `postgres/postgres` for username/password and creates `subscription_db` automatically.
- `backend/db/schema.sql` is applied automatically on first startup of a fresh DB volume.
- If you need to re-run initialization from scratch:

```bash
docker compose down -v
docker compose up -d db
```

### 2. Backend

```bash
cd backend
cp .env.example .env        # Fill in your DB credentials
npm install
npm run dev                 # Starts on http://localhost:5000
```

#### API Endpoints

| Method | Endpoint                    | Description               |
|--------|-----------------------------|---------------------------|
| GET    | `/api/subscriptions`        | List all subscriptions    |
| GET    | `/api/subscriptions/:id`    | Get a single subscription |
| POST   | `/api/subscriptions`        | Create a subscription     |
| PUT    | `/api/subscriptions/:id`    | Update a subscription     |
| DELETE | `/api/subscriptions/:id`    | Delete a subscription     |
| GET    | `/health`                   | Health check              |

#### Subscription Object

```json
{
  "name": "Netflix",
  "category": "Streaming",
  "cost": 15.99,
  "billing_cycle": "Monthly",
  "start_date": "2024-01-01",
  "renewal_date": "2026-04-01",
  "status": "Active",
  "description": "Family plan"
}
```

### 3. Frontend

```bash
cd frontend
cp .env.example .env        # Optional: set REACT_APP_API_URL
npm install
npm start                   # Starts on http://localhost:3000
```

## Environment Variables

### Backend (`backend/.env`)

| Variable      | Default           | Description       |
|---------------|-------------------|-------------------|
| `PORT`        | `5000`            | Server port       |
| `DB_HOST`     | `localhost`       | PostgreSQL host   |
| `DB_PORT`     | `5432`            | PostgreSQL port   |
| `DB_NAME`     | `subscription_db` | Database name     |
| `DB_USER`     | `postgres`        | Database user     |
| `DB_PASSWORD` | _(empty)_         | Database password |

### Frontend (`frontend/.env`)

| Variable            | Default                       | Description     |
|---------------------|-------------------------------|-----------------|
| `REACT_APP_API_URL` | `/api` (proxied to port 5000) | Backend API URL |

## License

[MIT](LICENSE)
