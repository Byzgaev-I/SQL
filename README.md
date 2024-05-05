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
Здесь:
- --name postgres-instance задает имя контейнера.  
- e POSTGRES_PASSWORD=mysecretpassword устанавливает пароль для пользователя postgres.  
- -d запускает контейнер в фоновом режиме.  
- -v /my/local/path/to/postgres/data:/var/lib/postgresql/data создает том для хранения данных БД между локальной папкой /my/local/path/to/postgres/data и папкой /var/lib/postgresql/data в контейнере.  
- -v /my/local/path/to/postgres/backups:/backups создает том для хранения бэкапов между локальной папкой /my/local/path/to/postgres/backups и папкой /backups в контейнере.  
postgres:12 указывает на использование образа PostgreSQL версии 12.  
