# Практическое задание к Уроку 10

## Цель работы
Научиться создавать функции для повторного использования логики и триггеры для автоматического обновления данных.

## Что нужно сделать

### Шаг 1. Создание функции
1. Напишите функцию `get_customer_total(customer_id INT)`, которая возвращает общую сумму заказов клиента.
   ~~~sql
   CREATE OR REPLACE FUNCTION get_customer_total(c_id INT)
   RETURNS DECIMAL AS $$
   BEGIN
       RETURN (SELECT SUM(amount) FROM orders WHERE customer_id = c_id);
   END;
   $$ LANGUAGE plpgsql;
   ~~~
2. Вызовите функцию для клиента с ID = 1:
   ~~~sql
   SELECT get_customer_total(1);
   ~~~
3. Убедитесь, что функция возвращает корректную сумму.

### Шаг 2. Создание триггера
Добавим в таблицу `orders` колонку для отслеживания даты изменения.
1. Добавьте колонку `updated_at`:
   ~~~sql
   ALTER TABLE orders ADD COLUMN updated_at TIMESTAMP;
   ~~~
2. Создайте функцию, которая обновляет это поле:
   ~~~sql
   CREATE OR REPLACE FUNCTION update_order_timestamp()
   RETURNS TRIGGER AS $$
   BEGIN
       NEW.updated_at = NOW();
       RETURN NEW;
   END;
   $$ LANGUAGE plpgsql;
   ~~~
3. Создайте триггер на обновление:
   ~~~sql
   CREATE TRIGGER order_updated
   BEFORE UPDATE ON orders
   FOR EACH ROW
   EXECUTE FUNCTION update_order_timestamp();
   ~~~

### Шаг 3. Проверка триггера
1. Обновите любой заказ (например, измените сумму):
   ~~~sql
   UPDATE orders SET amount = 10000 WHERE id = 1;
   ~~~
2. Проверьте, что поле `updated_at` заполнилось:
   ~~~sql
   SELECT * FROM orders WHERE id = 1;
   ~~~
3. Убедитесь, что дата соответствует текущему моменту.

---

## Ожидаемый результат
Вы умеете создавать функции, которые выполняются на сервере, и триггеры, которые автоматически реагируют на изменения данных.