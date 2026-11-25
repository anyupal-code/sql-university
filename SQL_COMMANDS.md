# Справочник команд SQL

## 1. Выборка данных (DQL)
~~~sql
-- Все колонки
SELECT * FROM table_name;

-- Конкретные колонки
SELECT column1, column2 FROM table_name;

-- Фильтрация (WHERE)
SELECT * FROM users WHERE age > 25;
SELECT * FROM users WHERE name LIKE 'A%'; -- Начинается на 'A'
SELECT * FROM users WHERE age BETWEEN 20 AND 30;
SELECT * FROM users WHERE id IN (1, 2, 3);

-- Сортировка (ORDER BY)
SELECT * FROM users ORDER BY age DESC; -- По убыванию
SELECT * FROM users ORDER BY name ASC; -- По возрастанию

-- Ограничение (LIMIT)
SELECT * FROM users LIMIT 10;
~~~

## 2. Изменение данных (DML)
~~~sql
-- Вставка (INSERT)
INSERT INTO users (name, age) VALUES ('Иван', 25);

-- Обновление (UPDATE)
UPDATE users SET age = 26 WHERE name = 'Иван';

-- Удаление (DELETE)
DELETE FROM users WHERE name = 'Иван';
~~~

## 3. Объединение таблиц (JOIN)
~~~sql
-- INNER JOIN (только совпадения)
SELECT a.name, b.product 
FROM customers a
INNER JOIN orders b ON a.id = b.customer_id;

-- LEFT JOIN (все из левой + совпадения из правой)
SELECT a.name, b.product 
FROM customers a
LEFT JOIN orders b ON a.id = b.customer_id;
~~~

## 4. Агрегация и группировка
~~~sql
-- Сумма, среднее, количество
SELECT SUM(amount) FROM orders;
SELECT AVG(amount) FROM orders;
SELECT COUNT(*) FROM orders;

-- Группировка (GROUP BY)
SELECT customer_id, SUM(amount) 
FROM orders 
GROUP BY customer_id;

-- Фильтрация групп (HAVING)
SELECT customer_id, SUM(amount) 
FROM orders 
GROUP BY customer_id
HAVING SUM(amount) > 50000;
~~~

## 5. Подзапросы и представления
~~~sql
-- Подзапрос в WHERE
SELECT name FROM customers 
WHERE id IN (SELECT customer_id FROM orders WHERE amount > 50000);

-- Создание представления (VIEW)
CREATE VIEW customer_totals AS
SELECT c.name, SUM(o.amount) as total
FROM customers c
JOIN orders o ON c.id = o.customer_id
GROUP BY c.name;

-- Использование VIEW
SELECT * FROM customer_totals WHERE total > 50000;
~~~

## 6. Транзакции
~~~sql
BEGIN; -- Начать транзакцию
UPDATE accounts SET balance = balance - 1000 WHERE id = 1;
UPDATE accounts SET balance = balance + 1000 WHERE id = 2;
COMMIT; -- Подтвердить

-- Или откатить
ROLLBACK;
~~~

## 7. Индексы и оптимизация
~~~sql
-- Создание индекса
CREATE INDEX idx_users_email ON users (email);

-- Анализ запроса
EXPLAIN ANALYZE SELECT * FROM users WHERE email = 'test@example.com';
~~~

## 8. Хранимые процедуры и триггеры
~~~sql
-- Создание функции
CREATE OR REPLACE FUNCTION get_total_orders(user_id INT)
RETURNS DECIMAL AS $$
BEGIN
    RETURN (SELECT SUM(amount) FROM orders WHERE customer_id = user_id);
END;
$$ LANGUAGE plpgsql;

-- Вызов функции
SELECT get_total_orders(1);
~~~

## 9. Управление структурой таблиц (DDL)
~~~sql
-- Создание таблицы
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    age INT
);

-- Добавление колонки
ALTER TABLE users ADD COLUMN email VARCHAR(100);

-- Удаление таблицы
DROP TABLE users;
~~~