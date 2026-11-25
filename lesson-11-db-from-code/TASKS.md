# Практическое задание к Уроку 11

## Цель работы
Написать скрипт, который подключается к базе данных, выводит данные и безопасно добавляет новую запись.

## Что нужно сделать

### Шаг 1. Установка библиотеки
* **Для Python:** выполните `pip install psycopg2-binary`.
* **Для Java:** добавьте зависимость на JDBC-драйвер PostgreSQL в `pom.xml` (если используете Maven).
* **Для C++:** установите `libpqxx`.

### Шаг 2. Написание скрипта (Python)
Создайте файл `db_script.py` и напишите скрипт:
1. Подключитесь к базе `mydb` с пользователем `myuser`.
2. Выполните запрос `SELECT name, email FROM customers;` и выведите результаты в консоль.
3. Безопасно добавьте нового клиента (например, 'Алексей', 'alexey@example.com') с помощью параметризованного запроса.

*Примерный каркас:*
~~~python
import psycopg2

try:
    conn = psycopg2.connect(
        host="localhost",
        database="mydb",
        user="myuser",
        password="mypassword"
    )
    cur = conn.cursor()
    
    # Чтение данных
    cur.execute("SELECT name, email FROM customers;")
    print("Клиенты:")
    for row in cur.fetchall():
        print(f"  {row[0]}, {row[1]}")
    
    # Безопасная вставка
    cur.execute(
        "INSERT INTO customers (name, email) VALUES (%s, %s)",
        ("Алексей", "alexey@example.com")
    )
    conn.commit()
    print("Новый клиент добавлен.")
    
    cur.close()
    conn.close()
except Exception as e:
    print(f"Ошибка: {e}")
~~~

### Шаг 3. Проверка
1. Запустите скрипт: `python db_script.py`.
2. Убедитесь, что список клиентов выводится и новый клиент добавился в базу.
3. В DBeaver выполните `SELECT * FROM customers;` и проверьте, что Алексей появился.

---

## Ожидаемый результат
Вы умеете подключаться к PostgreSQL из кода, выполнять запросы и безопасно передавать данные. Теперь вы можете создавать приложения, которые работают с реальной базой данных.