---
## 📚 Lesson #001
**Topic:** FastAPI Ecosystem - Commonly Used Libraries
**Date:** 2026-01-07
**Difficulty:** Intermediate
**Tags:** #fastapi #python #backend #web-framework #ecosystem
---

### 📌 Quick Summary
FastAPI is built on **Starlette** (ASGI) and **Pydantic** (validation). The production ecosystem includes SQLAlchemy/SQLModel for databases, Celery+Redis for background tasks, and pytest for testing.

### 🤔 Why It Matters
Choosing the right libraries upfront prevents technical debt and ensures your FastAPI application scales properly. The ecosystem has standardized around specific tools that integrate seamlessly with FastAPI's async-first architecture.

### 📖 Detailed Explanation

FastAPI's ecosystem can be organized into layers:

## 1. Core Dependencies (Auto-installed)

| Library | Purpose |
|---------|---------|
| **Pydantic** | Data validation, serialization, settings |
| **Starlette** | ASGI framework (routing, middleware, WebSockets) |
| **Uvicorn** | ASGI server (production: use with Gunicorn) |

## 2. Database Layer

| Library | Purpose | When to Use |
|---------|---------|-------------|
| **SQLAlchemy 2.0+** | Full-featured ORM | Complex queries, existing projects |
| **SQLModel** | SQLAlchemy + Pydantic fusion | New FastAPI projects (same author!) |
| **Alembic** | Database migrations | Always (schema versioning) |
| **asyncpg** | Async PostgreSQL driver | PostgreSQL + async |
| **aiosqlite** | Async SQLite driver | Development/testing |

## 3. Authentication & Security

| Library | Purpose |
|---------|---------|
| **python-jose[cryptography]** | JWT encoding/decoding |
| **passlib[bcrypt]** | Password hashing |
| **python-multipart** | Form data parsing (OAuth2 flows) |

## 4. Background Tasks & Caching

| Library | Purpose |
|---------|---------|
| **Celery** | Distributed task queue |
| **Redis** | Message broker + caching + session storage |
| **ARQ** | Async alternative to Celery (lighter) |

## 5. Testing

| Library | Purpose |
|---------|---------|
| **pytest** | Test framework |
| **pytest-asyncio** | Async test support |
| **httpx** | Async HTTP client (replaces requests for testing) |
| **factory-boy** | Test fixtures |

## 6. Configuration & Utilities

| Library | Purpose |
|---------|---------|
| **pydantic-settings** | Environment-based configuration |
| **python-dotenv** | .env file loading |
| **orjson** | Fast JSON serialization |
| **email-validator** | Email validation for Pydantic |

### 💻 Code Examples

#### Typical requirements.txt (Production Stack)
```txt
# Core
fastapi>=0.115.0
uvicorn[standard]>=0.32.0
gunicorn>=23.0.0

# Database
sqlalchemy[asyncio]>=2.0.0
asyncpg>=0.30.0
alembic>=1.14.0
sqlmodel>=0.0.22

# Authentication
python-jose[cryptography]>=3.3.0
passlib[bcrypt]>=1.7.4
python-multipart>=0.0.12

# Background Tasks
celery>=5.4.0
redis>=5.2.0

# Configuration
pydantic-settings>=2.6.0
python-dotenv>=1.0.0

# Testing (dev)
pytest>=8.3.0
pytest-asyncio>=0.24.0
httpx>=0.28.0

# Utilities
orjson>=3.10.0
email-validator>=2.2.0
```

#### Basic Project Structure
```
my_fastapi_app/
├── app/
│   ├── __init__.py
│   ├── main.py              # FastAPI app instance
│   ├── config.py            # pydantic-settings
│   ├── database.py          # SQLAlchemy/SQLModel setup
│   ├── models/              # SQLAlchemy/SQLModel models
│   ├── schemas/             # Pydantic schemas (if not using SQLModel)
│   ├── routers/             # API routes
│   ├── services/            # Business logic
│   ├── repositories/        # Data access layer
│   └── dependencies.py      # FastAPI dependencies
├── migrations/              # Alembic migrations
├── tests/
├── .env
├── requirements.txt
└── docker-compose.yml
```

#### Database Setup with SQLModel (Async)
```python
# app/database.py
from sqlalchemy.ext.asyncio import create_async_engine, AsyncSession
from sqlalchemy.orm import sessionmaker
from sqlmodel import SQLModel

from app.config import settings

engine = create_async_engine(
    settings.database_url,
    echo=settings.debug,
    pool_pre_ping=True,  # Health check connections
)

async_session = sessionmaker(
    engine, class_=AsyncSession, expire_on_commit=False
)

async def get_session() -> AsyncSession:
    async with async_session() as session:
        yield session

async def init_db():
    async with engine.begin() as conn:
        await conn.run_sync(SQLModel.metadata.create_all)
```

### ⚠️ Common Pitfalls

1. **Mixing sync/async drivers** - Use `asyncpg` not `psycopg2` with async FastAPI
2. **Forgetting `python-multipart`** - Required for OAuth2 form-based auth
3. **Using `requests` for testing** - Use `httpx` (async-compatible)
4. **SQLModel vs separate Pydantic schemas** - SQLModel combines both; choose one pattern
5. **Celery without proper broker** - Always use Redis or RabbitMQ, not SQLite

### 🎯 Practice Challenge

Create a `requirements.txt` and basic project structure for a FastAPI app that:
- Uses PostgreSQL with async SQLModel
- Has JWT authentication
- Includes Celery for background email sending
- Has pytest configured for async testing

### 🔗 Related Topics
- SQLModel deep dive
- Async SQLAlchemy patterns
- JWT authentication implementation
- Celery task patterns
- FastAPI testing strategies

### 🔍 Research Sources
**Last Updated:** 2026-01-07

**Sources:**
- [FastAPI SQL Databases Tutorial](https://fastapi.tiangolo.com/tutorial/sql-databases/)
- [SQLModel + FastAPI Tutorial](https://sqlmodel.tiangolo.com/tutorial/fastapi/)
- [Pydantic Documentation](https://docs.pydantic.dev/latest/)
- [Top Python Libraries 2025 - Tryolabs](https://tryolabs.com/blog/top-python-libraries-2025)
- [Async Tasks with FastAPI and Celery - TestDriven.io](https://testdriven.io/blog/fastapi-and-celery/)
- [awesome-fastapi - Curated List](https://github.com/mjhea0/awesome-fastapi)
- [JWT Authentication in FastAPI 2025](https://craftyourstartup.com/cys-docs/jwt-authentication-in-fastapi-guide/)

### ✅ Key Takeaways

- **Core trio**: FastAPI + Pydantic + Uvicorn (auto-installed)
- **Database**: SQLModel (new) or SQLAlchemy 2.0+ (existing) + Alembic for migrations
- **Auth**: python-jose + passlib + python-multipart
- **Background tasks**: Celery + Redis (production) or ARQ (lightweight)
- **Testing**: pytest + pytest-asyncio + httpx
- **Config**: pydantic-settings + python-dotenv

---
