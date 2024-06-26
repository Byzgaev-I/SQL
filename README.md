# Домашнее задание к занятию "`«SQL»`" - `Бызгаев Александр`

---

### Задание 1

Используя Docker, поднимите инстанс PostgreSQL (версию 12) c 2 volume, в который будут складываться данные БД и бэкапы.
Приведите получившуюся команду или docker-compose-манифест.

### Решение:

Чтобы поднять инстанс PostgreSQL с использованием Docker и двумя томами (один для данных БД, другой для бэкапов), использую следующую команду docker run:

```bash
docker run --name postgres12 -e POSTGRES_PASSWORD=Sasha123 -d \
  -v /my/local/path/to/postgres/data:/var/lib/postgresql/data \
  -v /my/local/path/to/postgres/backups:/backups \
  postgres:12
```
Здесь:
- --name postgres12 задает имя контейнера.  
- e POSTGRES_PASSWORD=Sasha123 устанавливает пароль для пользователя postgres.  
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
![image](https://github.com/Byzgaev-I/SQL/blob/main/1-3.png)

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

 ### Решение:

 - Создал пользователя test-admin-user с паролем.    
 - Создал пользователя test-simple-user с паролем.    
 - Создал базу данных test_db.    
 - Создал таблицу orders.    
 - Создал таблицу clients.    
 - Создал индекс для сountry_residence.    
 - Разрешил подключение пользователя test-admin-user к базе данных test_db.    
 - Выдал права на все таблица в схеме public для test-admin-user.    
 - Разрешил подключение пользователя test-simple-user к базе данных test_db.    
 - Выдал необходимые права на все таблица в схеме public для test-simple-user".    

- список БД после выполнения написанного:

![image](https://github.com/Byzgaev-I/SQL/blob/main/2-1.png)  

- описание таблиц (describe):

 ![image](https://github.com/Byzgaev-I/SQL/blob/main/2-2.png)

 - SQL-запрос для выдачи списка пользователей с правами над таблицами test_db:
```bash
test_db=# SELECT * FROM information_schema.table_privileges WHERE table_catalog = 'test_db' AND grantee LIKE 'test%';
``` 

 - список пользователей с правами над таблицами test_db:

![image](https://github.com/Byzgaev-I/SQL/blob/main/2-3.png)

---

### Задание 3

Используя SQL-синтаксис, наполните таблицы следующими тестовыми данными:  

Таблица orders 
| Наименование | Цена |
|--------------|------|
| Шоколад      | 10|  
| Принтер      | 3000 |   
| Книга        | 500|      
| Монитор      | 7000| 
| Гитара       | 4000|

Таблица clients

|ФИО| Страна проживания |
|--------------|------|
| Иванов Иван Иванович| USA|  
| Петров Петр Петрович | Canada |   
| Иоганн Себастьян Бах      | Japan|      
| Ронни Джеймс Дио     | Russia| 
| Ritchie Blackmore      | Russia|

Используя SQL-синтаксис:

- вычислите количество записей для каждой таблицы.
- 
- Приведите в ответе:
 
```bash
- запросы,
- результаты их выполнения.
``` 

 ### Решение:

```bash
Наполнил таблицу orders  
test_db=# INSERT INTO orders (id, title, price) VALUES (1, 'chocolate', 10), (2, 'printer', 3000), (3, 'book', 500), (4, 'monitor', 7000), (5, 'guitar', 4000);

Наполнил таблицу clients  
test_db=# INSERT INTO clients (id, second_name, сountry_residence) VALUES (1, 'Иванов Иван Иванович', 'USA'), (2, 'Петров Петр Петрович', 'Canada'), (3, 'Иоганн Себастьян Бах', 'Japan'), (4, 'Ронни Джеймс Дио', 'Russia'), (5, 'Ritchie Blackmore', 'Russia');  
```
![image](https://github.com/Byzgaev-I/SQL/blob/main/3.png)

На скрине видим вычисление количества записей для каждой таблицы
Запросы и результаты их выполнения.

![image](https://github.com/Byzgaev-I/SQL/blob/main/3-1.png)

---

### Задание 4

Часть пользователей из таблицы clients решили оформить заказы из таблицы orders.
Используя foreign keys, свяжите записи из таблиц, согласно таблице:

### Решение:
 
|ФИО| Заказ |
|--------------|------|
| Иванов Иван Иванович| Книга|  
| Петров Петр Петрович | Монитор |   
| Иоганн Себастьян Бах      | Гитара|   

Приведите SQL-запросы для выполнения этих операций.
Приведите SQL-запрос для выдачи всех пользователей, которые совершили заказ, а также вывод этого запроса.

![image](https://github.com/Byzgaev-I/SQL/blob/main/4.png)

---

### Задание 5

Часть пользователей из таблицы clients решили оформить заказы из таблицы orders.
Используя foreign keys, свяжите записи из таблиц, согласно таблице:
Получите полную информацию по выполнению запроса выдачи всех пользователей из задачи 4 (используя директиву EXPLAIN).
Приведите получившийся результат и объясните, что значат полученные значения.

### Решение:

![image](https://github.com/Byzgaev-I/SQL/blob/main/5.png)

- Чтение таблицы orders    
- Создание хеша для поля id  
- Сканирование таблицы clients
- Rows - приблизительно возвращаемое количество строк
- Для каждой строки по полю booking будет проверено, соответствует ли она чему-то в кеше orders  
- При наличии соответствий далее формируется вывод  
- Так же указано примерное количество строк, вес и стоимость.  

---

### Задание 6

Создайте бэкап БД test_db и поместите его в volume, предназначенный для бэкапов (см. задачу 1).  
Остановите контейнер с PostgreSQL, но не удаляйте volumes.  
Поднимите новый пустой контейнер с PostgreSQL.  
Восстановите БД test_db в новом контейнере.  
Приведите список операций, который вы применяли для бэкапа данных и восстановления.  

### Решение:

```bash
# Подключаюсь к контейнеру
docker exec -it postgres12 bash

# Создаю бекап на нужном volume
pg_dump -U postgres -W test_db > /var/lib/postgresql/backup/test_db.sql

# Останваливаю старый контейнер
docker stop postgres12

# Поднимаю новый контейнер и монтирую volume с бекапом
docker run --network host --name postgres12_test -e POSTGRES_PASSWORD=Sasha123 -ti -d -v vol2:/var/lib/postgresql/backup postgres:12

# Подключаюсь к новому контейнеру
docker exec -it postgres12_test bash

# Востанавливаю из бекапа необходимой базы
psql -U postgres -W test_db < /var/lib/postgresql/backup/test_db.sql

```











