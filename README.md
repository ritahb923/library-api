# Library API

**By:** Charity Munyao
C027-01-0865/2024

A simple library management REST API built with **FastAPI**, **SQLModel**, and **PostgreSQL**. Built as part of Lab 4: PostgreSQL & SQLModel.

## Features

- CRUD operations for books
- Category model with a one-to-many relationship to books
- Search books by author or title
- Partial updates via PATCH
- Auto-generated interactive API docs (Swagger UI)

## Tech Stack

- **FastAPI** – web framework
- **SQLModel** – ORM + Pydantic validation combined
- **PostgreSQL 16** – database (via Docker)
- **psycopg2-binary** – PostgreSQL driver
- **uv** – Python package/project manager
- **python-dotenv** – environment variable management

## Prerequisites

- Python 3.10+
- [uv](https://docs.astral.sh/uv/)
- Docker Desktop (with WSL2 or Hyper-V backend, if on Windows)

## Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/ritahb923/library-api.git
   cd library-api
   ```

2. **Install dependencies**
   ```bash
   uv sync
   ```

3. **Set up environment variables**

   Create a `.env` file in the project root:
   ```
   DATABASE_URL=postgresql://postgres:postgres@localhost:5432/library_db
   ```

4. **Start PostgreSQL**
   ```bash
   docker compose up -d
   ```
   Verify it's running:
   ```bash
   docker ps
   ```

5. **Run the API**
   ```bash
   uv run uvicorn main:app --reload
   ```

6. **Open the interactive docs**

   Visit [http://localhost:8000/docs](http://localhost:8000/docs) in your browser.


## API Endpoints

| Method | Endpoint             | Description                          |
|--------|-----------------------|---------------------------------------|
| GET    | `/`                   | Welcome message                       |
| POST   | `/books`              | Create a new book                     |
| GET    | `/books`              | List books (supports `skip`, `limit`, `available` filters) |
| GET    | `/books/search`       | Search books by `author` and/or `title` |
| GET    | `/books/{book_id}`    | Get a single book by ID               |
| PATCH  | `/books/{book_id}`    | Update a book (partial update)        |
| POST   | `/categories`         | Create a new category                 |

## Example Requests

**Create a category**
```
POST /categories?name=Fiction
```

**Create a book**
```json
POST /books
{
  "title": "The Great Gatsby",
  "author": "F. Scott Fitzgerald",
  "isbn": "9780743273565",
  "published_year": 1925,
  "category_id": 1
}
```

**Search books**
```
GET /books/search?author=Fitzgerald
GET /books/search?title=Gatsby
```

**Update a book**
```json
PATCH /books/1
{
  "title": "The Great Gatsby (Updated)",
  "available": false
}
```

## Database Access

To inspect the database directly:
```bash
docker exec -it library_db psql -U postgres -d library_db
```
Then, inside `psql`:
```sql
\dt
SELECT * FROM book;
SELECT * FROM category;
```
