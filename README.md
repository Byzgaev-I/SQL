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
- postgres:12 указывает на использование образа PostgreSQL версии 12.  

```bash
version: '3.8'

services:
  postgres:
    image: postgres:12
    container_name: postgres12
    restart: always
    ports:
      - 5432:5432
    environment:
      POSTGRES_DB: mydb
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: Sasha123
    volumes:
      - pg_data:/var/lib/postgresql/data
      - pg_backups:/backups

volumes:
  pg_data:
  pg_backups:
```

---

### Задание 2

В БД из задачи 1:
- создайте пользователя test-admin-user и БД test_db;  
- в БД test_db создайте таблицу orders и clients (спeцификация таблиц ниже);  
- предоставьте привилегии на все операции пользователю test-admin-user на таблицы БД test_db;  
- создайте пользователя test-simple-user;  
- предоставьте пользователю test-simple-user права на SELECT/INSERT/UPDATE/DELETE этих таблиц БД test_db.
  
Таблица orders:

- id (serial primary key);  
- наименование (string);  
- цена (integer).
  
Таблица clients:

- id (serial primary key);  
- фамилия (string);  
- страна проживания (string, index);  
- заказ (foreign key orders).
- 
Приведите:

- итоговый список БД после выполнения пунктов выше;  
- описание таблиц (describe);  
- SQL-запрос для выдачи списка пользователей с правами над таблицами test_db;  
- список пользователей с правами над таблицами test_db.

  























