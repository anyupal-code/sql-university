# Практическое задание к Уроку 1

## Цель работы
Установить PostgreSQL, создать пользователя и базу данных, подключиться через графический клиент DBeaver.

## Что нужно сделать

### Шаг 1. Установка PostgreSQL
Выберите инструкцию для вашей операционной системы.

**Ubuntu/Debian:**
~~~bash
sudo apt update
sudo apt install postgresql postgresql-contrib
~~~

**Fedora:**
~~~bash
sudo dnf install postgresql-server postgresql-contrib
sudo postgresql-setup --initdb
sudo systemctl start postgresql
sudo systemctl enable postgresql
~~~

**Windows/macOS:**
Скачайте установщик с официального сайта (postgresql.org) или используйте пакетный менеджер (Homebrew для macOS).

### Шаг 2. Создание пользователя и базы данных
1. Войдите в консоль PostgreSQL под суперпользователем:
   ~~~bash
   sudo -u postgres psql
   ~~~
2. Создайте нового пользователя (замените пароль на свой):
   ~~~sql
   CREATE USER myuser WITH PASSWORD 'mypassword';
   ~~~
3. Создайте базу данных для этого пользователя:
   ~~~sql
   CREATE DATABASE mydb OWNER myuser;
   ~~~
4. Выйдите из консоли:
   ~~~sql
   \q
   ~~~

### Шаг 3. Установка DBeaver
Скачайте и установите DBeaver с официального сайта (dbeaver.io). 
Для Linux доступна установка через пакетный менеджер (например: `sudo dnf install dbeaver`).

### Шаг 4. Подключение через DBeaver
1. Откройте DBeaver и нажмите **New Database Connection** (иконка с плюсом).
2. Выберите **PostgreSQL** и нажмите **Next**.
3. Введите данные:
   * **Host:** `localhost`
   * **Port:** `5432`
   * **Database:** `mydb`
   * **Username:** `myuser`
   * **Password:** (ваш пароль)
4. Нажмите **Test Connection**. При успехе нажмите **Finish**.

### Шаг 5. Проверка
1. В левой панели DBeaver раскройте подключение `mydb`.
2. Откройте SQL-редактор (F3 или иконка листа с плюсом).
3. Выполните запрос:
   ~~~sql
   SELECT version();
   ~~~
4. В окне результатов отобразится версия PostgreSQL.

---

## Ожидаемый результат
Установлен PostgreSQL, создан пользователь `myuser` и база данных `mydb`. Выполнено подключение через DBeaver.