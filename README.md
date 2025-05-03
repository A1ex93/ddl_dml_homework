# Домашнее задание к занятию "`DDL_DML_Homework`" - `Gurylev A.V.`


# Задание 1

1.1. Поднимите чистый инстанс MySQL версии 8.0+. Можно использовать локальный сервер или контейнер Docker.

# Установка MySQL

    ![alt text](https://github.com/A1ex93/ddl_dml_homework/blob/main/image/1.1.png?raw=true)

    docker run --name mysql8 -e MYSQL_ROOT_PASSWORD=root -p 3306:3306 -d mysql:8.0

# Подключение к контейнеру

    docker exec -it mysql8 mysql -u root -p

1.2. Создайте учётную запись sys_temp.

    ![alt text](https://github.com/username/reponame/blob/branch/path/image.png)

    CREATE USER 'sys_temp' IDENTIFIED BY 'password123';

1.3. Выполните запрос на получение списка пользователей в базе данных. (скриншот)

    SELECT User, Host FROM mysql.user;

1.4. Дайте все права для пользователя sys_temp.

GRANT ALL PRIVILEGES ON *.* TO 'sys_temp'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;

1.5. Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)

SHOW GRANTS FOR 'sys_temp';

1.6. Переподключитесь к базе данных от имени sys_temp.

docker exec -it mysql8 mysql -u sys_temp -p

Для смены типа аутентификации с sha2 используйте запрос:

ALTER USER 'sys_test'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';

1.6. По ссылке https://downloads.mysql.com/docs/sakila-db.zip скачайте дамп базы данных.

1.7. Восстановите дамп в базу данных.

	1. Скопируй дамп внутрь контейнера
		docker cp backup.sql mysql_container:/backup.sql
	2. Восстановить дамп в нужную базу 
		docker exec -i mysql8 mysql -u root -p -D db_name -e "source /backup.sql"
		

1.8. При работе в IDE сформируйте ER-диаграмму получившейся базы данных. При работе в командной строке используйте команду для получения всех таблиц базы данных. (скриншот)
	
	USE your_database_name;
	SHOW TABLES;

	SHOW DATABASES;

Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.

# Задание 2
Составьте таблицу, используя любой текстовый редактор или Excel, в которой должно быть два столбца: в первом должны быть названия таблиц восстановленной базы, во втором названия первичных ключей этих таблиц. Пример: (скриншот/текст)

Название таблицы | Название первичного ключа
customer         | customer_id

SELECT
    t.table_name,
    c.column_name AS primary_key
FROM
    information_schema.table_constraints tc
JOIN
    information_schema.key_column_usage kcu
    ON tc.constraint_name = kcu.constraint_name
    AND tc.table_schema = kcu.table_schema
JOIN
    information_schema.columns c
    ON c.table_schema = tc.table_schema
    AND c.table_name = tc.table_name
    AND c.column_name = kcu.column_name
JOIN
    information_schema.tables t
    ON t.table_schema = tc.table_schema
    AND t.table_name = tc.table_name
WHERE
    tc.constraint_type = 'PRIMARY KEY'
    AND tc.table_schema = 'sakila';






