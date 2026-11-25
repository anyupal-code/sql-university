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

---

## Решение проблем при подключении (Troubleshooting)

### Ошибка: «пользователь не прошел проверку подлинности (ident)»
**Причина:** В Fedora по умолчанию для локальных подключений используется метод `ident`. Он требует, чтобы имя системного пользователя Linux совпадало с именем пользователя базы данных. DBeaver подключается по TCP (localhost), и сервер применяет это правило.

**Решение (для Fedora/Linux):**
1. Откройте файл конфигурации PostgreSQL:
   ~~~bash
   sudo nano /var/lib/pgsql/data/pg_hba.conf
   ~~~
2. Найдите строку, которая начинается с `host all all 127.0.0.1/32`. 
3. Замените метод `ident` на `md5`. Должно получиться:
   ~~~text
   host    all             all             127.0.0.1/32            md5
   ~~~
4. Сохраните файл (`Ctrl+O`, `Enter`, `Ctrl+X`).
5. Перезагрузите конфигурацию:
   ~~~bash
   sudo systemctl reload postgresql
   ~~~
6. В DBeaver нажмите **Test Connection** снова.

### Ошибка: «password authentication failed» (неверный пароль)
**Причина:** Пароль пользователя `myuser` был задан неверно или не был установлен.
**Решение:**
1. Войдите в консоль под суперпользователем:
   ~~~bash
   sudo -u postgres psql
   ~~~
2. Сбросьте пароль:
   ~~~sql
   ALTER USER myuser WITH PASSWORD 'mypassword';
   ~~~
3. Выйдите: `\q`.
4. В DBeaver введите пароль `mypassword`.