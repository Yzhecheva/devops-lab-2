# Runbook

> Fill in every TODO with exact commands. / Попълнете всяко TODO с точни команди.

## Start locally / Локално стартиране

TODO

## Check that it works / Проверка, че работи

TODO: which address do you open and what should you see? / кой адрес отваряте и какво трябва да видите?

## Run the tests / Тестове

TODO

## When something breaks / Когато нещо се счупи

TODO: the first three things you check. / първите три неща, които проверявате.


## Docker / Стартиране с Docker

### Build image / Изграждане на образ

```bash
docker build -t notes-api:secure .
```

### Run container / Стартиране на контейнер

```bash
docker run -d --name notes-api-secure -p 8000:8000 notes-api:secure
```

### Verify / Проверка

```bash
docker ps
curl http://localhost:8000/health
docker exec notes-api-secure whoami
```

Expected results:
- Container status: `healthy`.
- `/health` returns `{"status":"ok"}`.
- `whoami` returns `appuser`.

### Stop and remove / Спиране и премахване

```bash
docker stop notes-api-secure
docker rm notes-api-secure
```
