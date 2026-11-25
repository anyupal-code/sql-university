# Практическое задание к Уроку 4

## Цель работы
Научиться создавать связанные таблицы и извлекать данные из нескольких таблиц с помощью оператора JOIN.

## Что нужно сделать

### Шаг 1. Создание связанных таблиц
Откройте DBeaver и выполните скрипт для создания двух связанных таблиц:

~~~sql
-- Таблица клиентов
CREATE TABLE customers (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE
);

-- Таблица заказов (ссылается на клиентов)
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    customer_id INT REFERENCES customers(id),
    product VARCHAR(100),
    amount DECIMAL(10, 2)
);

-- Добавление клиентов
INSERT INTO customers (name, email) VALUES 
('Иван', 'ivan@example.com'),
('Мария', 'maria@example.com'),
('Петр', 'petr@example.com');

-- Добавление заказов (Иван и Мария сделали заказы, Петр — нет)
INSERT INTO orders (customer_id, product, amount) VALUES 
(1, 'Ноутбук', 75000.00),
(1, 'Мышь', 1500.00),
(2, 'Книга', 2000.00);
~~~

### Шаг 2. Выполнение запросов
Напишите и выполните следующие запросы:

1. **INNER JOIN:** Выведите имена клиентов и названия их товаров (только те, у кого есть заказы).
2. **LEFT JOIN:** Выведите имена **всех** клиентов и их заказы. Для тех, у кого нет заказов (Петр), в колонке товара должно быть `NULL`.
3. **Агрегация с JOIN:** Выведите имя клиента и общую сумму его заказов (используйте `SUM(amount)` и `GROUP BY`).

*Подсказка для третьего запроса:*
~~~sql
SELECT c.name, SUM(o.amount) as total
FROM customers c
JOIN orders o ON c.id = o.customer_id
GROUP BY c.name;
~~~

---

## Ожидаемый результат
Вы умеете связывать таблицы и извлекать данные из нескольких источников. Вы понимаете разницу между `INNER JOIN` (только совпадения) и `LEFT JOIN` (все из левой таблицы).