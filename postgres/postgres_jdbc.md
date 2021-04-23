
#________Установка и настройка postgres jdbc 

Переходим по ссылке 
https://jdbc.postgresql.org/download.html

Скачиваем в папку с найфаем. И оставляем там. Запоминаем путь - он нам понадобиться при настройке коннектора

```
wget https://jdbc.postgresql.org/download/postgresql-42.2.18.jar
```


Я его закинул вот сюда и переименовал для удобства: 

```
/home/uadmin/NiFi/postgres_driver_jdbc/42_18.jar
```

#--------Настройка/Установка Postgres на поиграть

У нас уже есть постгрес после установки AirFlow:
```
sudo apt-get install postgresql
sudo apt-get install build-essential libssl-dev libffi-dev python3-dev
sudo apt-get install libpq-dev

sudo -i -u postgres
drop database airflow;
create database airflow;
CREATE ROLE admin WITH SUPERUSER CREATEDB CREATEROLE LOGIN ENCRYPTED PASSWORD 'admin';
grant all privileges on database airflow to admin;
\q
exit
```

Достаточно первой строки:

```

sudo apt-get install postgresql postgresql-client postgresql-contrib
sudo apt-get install build-essential libssl-dev libffi-dev python3-dev
sudo apt-get install libpq-dev
```

Создадим теперь пользователя и базу данных для NiFi, и таблицу на поиграть:
```
sudo -i -u postgres
createdb nifi_db
psql
\conninfo
CREATE ROLE admin WITH SUPERUSER CREATEDB CREATEROLE LOGIN ENCRYPTED PASSWORD 'admin'
grant all privileges on database admin to admin

CREATE TABLE repository(id serial PRIMARY KEY, fname VARCHAR(50), email VARCHAR(50));
```

#________Путь к базе данных test. 
При настройке процессора следует обратить внимание на синтаксис - как указывать пути к базе данных

```
jdbc:postgresql://127.0.0.1:5432/nifi_db
```
Адрес: 127.0.0.1
Порт: 5432
База данных: nifi_db
логин: admin
пароль: admin
Имя класса драйвера: org.postgresql.Driver
Расположение: /home/uadmin/NiFi/postgres_driver_jdbc/42_18.jar

Если вдруг наше "поиграть" убежит в продакшн - не забываем постгрес причесать:
https://amkolomna.ru/content/ustanovka-i-nastroyka-servera-postgresql-pod-linux-ubuntu

#________Navicat (Для извращенцев и фанатов вьюшек)

```
wget http://download3.navicat.com/download/navicat112_premium_en_x64.tar.gz
tar -xvf navicat112_premium_en_x64.tar.gz
sudo chmod 775 start_navicat
./startnavicat
```

#________pgadmin4 (Для того чтобы нормально посмотреть базу постгреса)

Просто пошагово делаем вот так и потом оно у нас появится в окне приложений:
```
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ $(lsb_release -cs)-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'

sudo apt install wget ca-certificates

wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add

sudo apt update

sudo apt install pgadmin4
```

Источник:

https://betacode.net/11353/install-pgadmin-on-ubuntu
