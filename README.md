# User Management & Task Tracker API

A backend REST API built with **FastAPI** and **SQLite** for user management and task tracking.

## Tech Stack
- **Framework:** FastAPI (Python)
- **Database:** SQLite + SQLAlchemy ORM
- **Auth:** JWT (JSON Web Tokens)
- **Password Hashing:** bcrypt via passlib
- **Testing:** pytest + httpx

---

## Setup

### 1. Clone the repository
```bash
git clone https://github.com/EnjamYamini/task-tracker-api.git
cd task-tracker-api
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Configure environment variables
```bash
cp .env.example .env
```
Edit `.env` and set your own `SECRET_KEY`.

### 4. Run the server
```bash
uvicorn src.main:app --reload
```

Server runs at: `http://localhost:8000`

Interactive API docs: `http://localhost:8000/docs`

---

## Run Tests
```bash
pytest tests/ -v
```

---

## API Endpoints

### Authentication
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/auth/register` | Register new user |
| POST | `/auth/login` | Login and get JWT token |

### Users (Admin only)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/users/me` | Get own profile |
| GET | `/users/` | Get all users (admin) |
| DELETE | `/users/{id}` | Delete user (admin) |

### Tasks
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/tasks/` | Create task |
| GET | `/tasks/` | Get tasks (own / all for admin) |
| PUT | `/tasks/{id}` | Update task |
| DELETE | `/tasks/{id}` | Delete task |

---

## Sample API Requests

### Register
```bash
curl -X POST http://localhost:8000/auth/register \
  -H "Content-Type: application/json" \
  -d '{"username": "yamini", "email": "yamini@example.com", "password": "secret123"}'
```

### Login
```bash
curl -X POST http://localhost:8000/auth/login \
  -H "Content-Type: application/json" \
  -d '{"username": "yamini", "password": "secret123"}'
```

### Create Task (use token from login)
```bash
curl -X POST http://localhost:8000/tasks/ \
  -H "Authorization: Bearer YOUR_TOKEN_HERE" \
  -H "Content-Type: application/json" \
  -d '{"title": "My Task", "description": "Details here", "status": "pending"}'
```

### Get My Tasks
```bash
curl http://localhost:8000/tasks/ \
  -H "Authorization: Bearer YOUR_TOKEN_HERE"
```

---

## Security
- Passwords are hashed using **bcrypt**
- All protected routes require a valid **JWT token**
- Role-based access control (**admin** vs **user**)
- Secrets stored in **environment variables** (never in code)
