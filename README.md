# Домашнее задание к занятию "`«SQL»`" - `Бызгаев Александр`

---

### Задание 1

Используя Docker, поднимите инстанс PostgreSQL (версию 12) c 2 volume, в который будут складываться данные БД и бэкапы.
Приведите получившуюся команду или docker-compose-манифест.

### Решение:

Чтобы поднять инстанс PostgreSQL с использованием Docker и двумя томами (один для данных БД, другой для бэкапов), вы можете использовать следующую команду docker run:

```bash
docker run --name postgres-instance -e POSTGRES_PASSWORD=mysecretpassword -d \
  -v /my/local/path/to/postgres/data:/var/lib/postgresql/data \
  -v /my/local/path/to/postgres/backups:/backups \
  postgres:12
```
