# Практическое задание к Уроку 12

## Цель работы
Спроектировать и создать базу данных для интернет-магазина, наполнить её данными и выполнить аналитические запросы.

## Что нужно сделать

### Шаг 1. Создание схемы БД
Выполните скрипт для создания таблиц:

~~~sql
-- Клиенты
CREATE TABLE customers (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    registered_at TIMESTAMP DEFAULT NOW()
);

-- Товары
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    category VARCHAR(50),
    price DECIMAL(10, 2) NOT NULL,
    stock INT DEFAULT 0
);

-- Заказы
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    customer_id INT REFERENCES customers(id),
    order_date TIMESTAMP DEFAULT NOW(),
    status VARCHAR(20) DEFAULT 'new'
);

-- Позиции заказа
CREATE TABLE order_items (
    id SERIAL PRIMARY KEY,
    order_id INT REFERENCES orders(id),
    product_id INT REFERENCES products(id),
    quantity INT NOT NULL,
    price DECIMAL(10, 2) NOT NULL
);
~~~

### Шаг 2. Индексы для оптимизации
Создайте индексы для часто используемых колонок:

~~~sql
CREATE INDEX idx_customers_email ON customers(email);
CREATE INDEX idx_orders_status ON orders(status);
CREATE INDEX idx_products_category ON products(category);
~~~

### Шаг 3. Наполнение данными
Добавьте тестовые данные:

~~~sql
-- Клиенты
INSERT INTO customers (name, email) VALUES 
('Иван', 'ivan@example.com'),
('Мария', 'maria@example.com'),
('Петр', 'petr@example.com');

-- Товары
INSERT INTO products (name, category, price, stock) VALUES 
('Ноутбук', 'Электроника', 75000.00, 10),
('Смартфон', 'Электроника', 50000.00, 25),
('Книга SQL', 'Книги', 1500.00, 50),
('Наушники', 'Электроника', 3000.00, 0),
('Стол', 'Мебель', 15000.00, 5);

-- Заказы
INSERT INTO orders (customer_id, status) VALUES 
(1, 'paid'),
(1, 'delivered'),
(2, 'new'),
(3, 'paid');

-- Позиции заказов
INSERT INTO order_items (order_id, product_id, quantity, price) VALUES 
(1, 1, 1, 75000.00),
(1, 2, 1, 50000.00),
(2, 3, 2, 1500.00),
(3, 5, 1, 15000.00),
(4, 1, 1, 75000.00);
~~~

### Шаг 4. Аналитические запросы
Напишите следующие запросы:

1. **Общая выручка:** Выведите общую сумму всех заказов (сумма `price * quantity` из `order_items`).
2. **Топ-3 товара по продажам:** Выведите названия товаров и их общее количество продаж (отсортировать по убыванию).
3. **Выручка по категориям:** Выведите категорию и общую выручку по ней.
4. **Активные клиенты:** Выведите имена клиентов и количество их заказов (только те, у кого больше 1 заказа).
5. **Средний чек:** Выведите среднюю сумму заказа (среднее по сумме позиций в каждом заказе).

---

## Ожидаемый результат
Вы создали полноценную базу данных для интернет-магазина. Вы умеете проектировать схему, создавать связи, оптимизировать запросы и извлекать аналитику.