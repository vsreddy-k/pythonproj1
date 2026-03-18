# Employee Management System

A production-ready **Flask + SQLite** web application with full CRUD, REST API, Docker support, and GitHub Actions CI/CD.

---

## Features

- **Web UI** — List, add, edit, delete, search, and filter employees (Bootstrap 5)
- **REST API** — Full JSON API for all employee operations
- **SQLite** — Zero-config embedded database (persisted via Docker volume)
- **Docker** — Multi-stage Dockerfile + docker-compose
- **GitHub Actions** — Automated testing (Python 3.10–3.12) and Docker Hub push on merge to `main`
- **Flask-Migrate** — Database schema migrations with Alembic

---

## Project Structure

```
employeemgnt/
├── app/
│   ├── __init__.py          # App factory
│   ├── models.py            # SQLAlchemy Employee model
│   ├── routes.py            # UI + REST API routes
│   └── templates/           # Jinja2 HTML templates
│       ├── base.html
│       ├── index.html
│       ├── form.html
│       └── view.html
├── tests/
│   └── test_app.py          # Pytest test suite (17 tests)
├── .github/
│   └── workflows/
│       └── ci-cd.yml        # GitHub Actions pipeline
├── instance/                # SQLite DB lives here (git-ignored)
├── run.py                   # Entry point
├── requirements.txt
├── Dockerfile               # Multi-stage production build
├── docker-compose.yml
├── pytest.ini
├── .gitignore
└── .env.example
```

---

## Quick Start (Local)

```bash
# 1. Clone and enter project
git clone https://github.com/<your-username>/employeemgnt.git
cd employeemgnt

# 2. Create virtual environment
python -m venv .venv
.venv\Scripts\activate        # Windows
# source .venv/bin/activate   # Linux/Mac

# 3. Install dependencies
pip install -r requirements.txt

# 4. Copy env file
copy .env.example .env        # Windows
# cp .env.example .env        # Linux/Mac

# 5. Run
python run.py
```

Open → http://localhost:5000

---

## Docker

```bash
# Build and run with docker-compose
docker-compose up --build

# Or plain Docker
docker build -t employeemgnt .
docker run -p 5000:5000 -v emp_data:/app/instance employeemgnt
```

Open → http://localhost:5000

---

## Run Tests

```bash
pytest tests/ -v
```

---

## REST API Reference

| Method   | Endpoint                   | Description          |
|----------|----------------------------|----------------------|
| `GET`    | `/api/employees`           | List all employees   |
| `GET`    | `/api/employees/<id>`      | Get one employee     |
| `POST`   | `/api/employees`           | Create employee      |
| `PUT`    | `/api/employees/<id>`      | Update employee      |
| `DELETE` | `/api/employees/<id>`      | Soft-delete employee |
| `GET`    | `/health`                  | Health check         |

### Create Employee (POST)
```json
{
  "name": "Subba Reddy",
  "email": "subba@example.com",
  "department": "Engineering",
  "position": "Developer",
  "salary": 75000,
  "phone": "9876543210",
  "hire_date": "2024-01-15"
}
```

---

## GitHub Actions Setup

1. Push the project to GitHub
2. Go to **Settings → Secrets and variables → Actions**
3. Add two secrets:
   - `DOCKERHUB_USERNAME` — your Docker Hub username
   - `DOCKERHUB_TOKEN` — your Docker Hub access token

The pipeline will:
- Run tests on Python 3.10, 3.11, and 3.12 on every push/PR
- Build and push a Docker image to Docker Hub on every merge to `main`

---

## Database Migrations

```bash
# Initialize (first time only)
flask db init

# Create a migration after model changes
flask db migrate -m "describe your change"

# Apply migrations
flask db upgrade
```

---

## Environment Variables

| Variable       | Default                          | Description                  |
|----------------|----------------------------------|------------------------------|
| `SECRET_KEY`   | `dev-secret-key-change-in-prod`  | Flask secret key             |
| `DATABASE_URL` | `sqlite:///instance/employees.db`| SQLAlchemy connection string |
| `FLASK_ENV`    | `production`                     | Flask environment            |

---

## License

MIT
