
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
