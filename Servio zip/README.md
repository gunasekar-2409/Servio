# SERVIO — Full-Stack Local Service Marketplace

A complete, database-connected full-stack web application connecting customers with independent local service providers in Tiruchirappalli.

**Institution**: SRMIST, Tiruchirappalli  
**Architecture**: 
- **Frontend**: HTML5, CSS3, Vanilla JavaScript (zero build step, Wix Handyman aesthetic)
- **Backend**: Python 3.12, FastAPI, SQLAlchemy 2.0, Pydantic v2
- **Database**: SQLite with relational schema, foreign keys, constraints, and timestamps
- **Security**: JWT Authentication (HS256) & salted PBKDF2-SHA256 password hashing

---

## Quick Start (Run Locally)

The application consists of two components running locally:
1. **FastAPI Backend**: `http://127.0.0.1:8000` (API & Swagger Docs at `/docs`)
2. **Frontend Web Server**: `http://127.0.0.1:8765`

### 1. Start the Backend API (FastAPI)

In a PowerShell terminal inside the project directory:

```powershell
# Activate virtual environment
.\backend\venv\Scripts\Activate.ps1

# Run the FastAPI server
uvicorn backend.app.main:app --host 127.0.0.1 --port 8000 --reload
```

*The database (`servio.db`) initializes and seeds automatically on first run via `backend/seed_data.py`.*

### 2. Start the Frontend Server

In a second PowerShell terminal inside the project directory:

```powershell
powershell -ExecutionPolicy Bypass -File .\serve.ps1
```

### 3. Open in Google Chrome

Visit:
👉 **[http://127.0.0.1:8765/](http://127.0.0.1:8765/)**

---

## Demonstration Accounts

The database comes pre-seeded with verified test accounts:

| Role | Name | Email | Password |
| --- | --- | --- | --- |
| **Customer** | Priya Menon | `priya@servio.demo` | `DemoPass123` |
| **Provider (Plumbing)** | Ravi Kumar | `ravi.plumber@servio.demo` | `DemoPass123` |
| **Provider (Electrical)** | Meena Selvam | `meena.electric@servio.demo` | `DemoPass123` |
| **Provider (Cleaning)** | Anita Joseph | `anita.clean@servio.demo` | `DemoPass123` |
| **Provider (Carpentry)** | Karthik N. | `karthik.wood@servio.demo` | `DemoPass123` |
| **Provider (Painting)** | Divya Ramesh | `divya.paint@servio.demo` | `DemoPass123` |

*You can also register brand-new Customer and Provider accounts via the Sign Up page.*

---

## Running Automated Tests

A comprehensive suite of 18 automated tests covers registration, authentication, permissions, service discovery, availability calculation, booking conflicts, state transitions, and reviews:

```powershell
& ".\backend\venv\Scripts\pytest.exe" -v backend\tests
```

Expected output:
```
backend/tests/test_auth.py ................ PASSED
backend/tests/test_bookings.py ............ PASSED
backend/tests/test_e2e_fullstack.py ....... PASSED
backend/tests/test_providers.py ........... PASSED
backend/tests/test_reviews.py ............. PASSED
backend/tests/test_services.py ............ PASSED

======================== 18 passed in 1.28s ========================
```

---

## Key Full-Stack Features

1. **Service Discovery & Search**:
   - Live database filtering by keyword, category, service area, and price range.
   - Authoritative pricing retrieved from the database.

2. **Booking Engine & Conflict Prevention**:
   - Validates that appointment dates are in the future.
   - Enforces provider weekly schedules and defined appointment slots.
   - Prevents overlapping reservations with atomic conflict checks (`HTTP 409 Conflict`).
   - Strict status workflow (`pending` -> `accepted` / `rejected` -> `completed` / `cancelled`).

3. **Provider Dashboard**:
   - Manage service listings (Add, Edit, Deactivate, Delete).
   - Real-time booking requests approval / rejection.
   - Manage availability schedules (days of week and daily time slots).
   - View true earnings derived from genuine completed bookings.

4. **Customer Dashboard**:
   - Real-time booking tracking and appointment status updates.
   - Cancellation of eligible pending/accepted appointments.
   - Verified review submissions enabled strictly after service completion.

5. **Security & Authorization**:
   - Passwords securely hashed with PBKDF2-SHA256 and salt.
   - Role-based access control (RBAC) enforced on every protected backend route.
   - Prevents privilege escalation and unauthorized service/booking modifications.
