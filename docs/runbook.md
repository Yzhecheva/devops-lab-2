
# Runbook

## Start locally / Локално стартиране

Install the project dependencies:

```bash
pip install -r requirements.txt
```

Start the application:

```bash
uvicorn app.main:app --host 0.0.0.0 --port 8000
```

## Check that it works / Проверка, че работи

Open `http://localhost:8000/health` in the browser.

Expected response:

```json
{"status":"ok"}
```

## Run the tests / Тестове

Install development dependencies and run the tests:

```bash
pip install -r requirements-dev.txt
pytest
```

## When something breaks / Когато нещо се счупи

1. Check whether the application or container is running.
2. Check the application logs for errors.
3. Check the dependencies, port configuration and `/health` endpoint.

## Docker / Стартиране с Docker

### Build image / Изграждане на образ

```bash
docker build -t notes-api:0.3.0 .
```

### Run container / Стартиране на контейнер

```bash
docker run -d --name notes-api -p 8000:8000 notes-api:0.3.0
```

### Check logs / Проверка на логовете

```bash
docker logs notes-api
```

### Verify / Проверка

```bash
docker ps
curl http://localhost:8000/health
docker exec notes-api whoami
docker inspect --format '{{.State.Health.Status}}' notes-api
```

Expected results:
- `/health` returns `{"status":"ok"}`.
- `whoami` returns `appuser`.
- Docker health status is `healthy`.

### Stop and remove / Спиране и премахване

```bash
docker stop notes-api
docker rm notes-api
```

## Docker Compose / Упражнение 4

### Configuration / Конфигурация

The application uses three services:
- `api` — FastAPI application on port 8000.
- `db` — PostgreSQL 16 database on port 5432.
- `adminer` — database administration interface on port 8080.

Create a local environment file:

```bash
cp .env.example .env
```

The `.env` file contains local configuration and is excluded from Git. The `.env.example` file provides example values.

### Start services / Стартиране

```bash
docker compose up -d --build
```

### Check services / Проверка

```bash
docker compose ps
curl http://localhost:8000/health
curl http://localhost:8000/ready
curl http://localhost:8000/version
```

The API and database should report `healthy`. The `/ready` endpoint should return `{"status":"ready"}`, and `/version` should return `{"version":"0.4.0"}`.

### Database / База данни

The API connects to PostgreSQL using the hostname `db`, which Docker Compose resolves through its internal network.

To view stored notes:

```bash
docker compose exec db psql -U notes -d notes -c "SELECT * FROM notes;"
```

### Adminer

Open port `8080` in the browser.

Connection settings:
- System: PostgreSQL
- Server: `db`
- Username: `notes`
- Password: value of `POSTGRES_PASSWORD` in `.env`
- Database: `notes`

### Data persistence / Запазване на данните

PostgreSQL stores its data in the named volume `postgres_data`.

```bash
docker compose down
docker compose up -d
```

The stored notes remain available after restarting the services.

**Warning:** `docker compose down -v` also removes the database volume and its stored data.

### Troubleshooting / Отстраняване на проблеми

Check service status:

```bash
docker compose ps
```

Check logs:

```bash
docker compose logs --tail=50 api
docker compose logs --tail=50 db
docker compose logs --tail=50 adminer
```

Validate configuration:

```bash
docker compose config --quiet
```

If the API cannot connect to PostgreSQL, check `DATABASE_URL`, database credentials and the `db` healthcheck.

### Stop services / Спиране

```bash
docker compose down
```
