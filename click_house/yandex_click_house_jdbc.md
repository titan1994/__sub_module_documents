#________Установка и настройка яндекс кликхаус на поиграть с NiFi 
Для тестирования http интерфейса


##_______Установка 

Переходим по ссылке:
 
https://clickhouse.tech/docs/en/getting-started/tutorial/

Делаем как там: 
```
sudo apt-get install apt-transport-https ca-certificates dirmngr
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv E0C56BD4

echo "deb https://repo.clickhouse.tech/deb/stable/ main/" | sudo tee /etc/apt/sources.list.d/clickhouse.list

sudo apt-get update
sudo apt-get install -y clickhouse-server clickhouse-client

cd ~
sudo /etc/init.d/clickhouse-server start

clickhouse-client

CREATE DATABASE IF NOT EXISTS tutorial

```


##________Пробник Data Grip

```
wget https://download.jetbrains.com/toolbox/jetbrains-toolbox-1.19.7784.tar.gz?_ga=2.256326987.1912744920.1611315326-751526815.1611315326
```

При коннекте 
пользователь: default
пароль не нужен 
база данных: default


##________Установка Maven (Для компиляции JDBC. Пропускаем-Уже поставлен)
Вот тут написано что нужен чудо-мейвен. 

https://nifi.apache.org/quickstart.html

Ставим:
```
sudo apt-get install maven
```

##________Устновка JAVA  (Для компиляции JDBC. Пропускаем-Уже поставлен). Обязательно отредактировать etc/profile
Нужен 8й JDK. Но можно и 11й. Я поставил 11й

```
sudo apt-get update
sudo apt-get install openjdk-8-jdk
sudo apt-get install openjdk-11-jdk-headless
```

Для джавы надо вот что ОБЯЗАТЕЛЬНО!!!
Этой командой найти путь:

```
update-alternatives --config java
```

Затем создать-добвить переменную среды JAVA_HOME в файл etc/profile и ребутнуться

В файл через любой удобный вам редактор пишем в корень вот такое вот 
(а путь, чему JAVA_HOME=, вы увидели в предыдущей команде):
```
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
export PATH=$PATH:$JAVA_HOME/bin

```
Закрываем редактор и перезагружаемся!

Проверка:
```
env | grep JAVA_HOME
java -version
```

Описано тут:
https://techexpert.tips/ru/%D0%B0%D0%BF%D0%B0%D1%87-%D0%BD%D0%B8%D1%84%D0%B8/apache-nifi-%D1%83%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0-%D0%BD%D0%B0-ubuntu-linux/

##________Компиляция драйвера JDBC 

Нужно через мавен скомпилить дрова для кликхауса. Со всеми  зависимостями,
потому что драйвер не stand-alone. И всё что там накомпилируется должно там и остаться
  
В компиляции есть ошибка, pom файл не корректный. 

Шаг1. Качаем (в туда где лежит найфай естественно):

```
git clone https://github.com/ClickHouse/clickhouse-jdbc 1 
```

Шаг2. правим pom.xml:
```
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-javadoc-plugin</artifactId>
    <version>3.0.1</version>
    <configuration>
        <javadocExecutable>${java.home}/bin/javadoc</javadocExecutable>
    </configuration>
</plugin>
```     
     
Было прям добавлено:

```
<configuration>
    <javadocExecutable>${java.home}/bin/javadoc</javadocExecutable>
</configuration>
```

И только потом собираем проект:
```
sudo mvn package assembly:single -DskipTests=true
```

И получаем заветный путь для найфая:

```
/home/uadmin/NiFi/1/target/clickhouse-jdbc-0.2.4-jar-with-dependencies.jar
jdbc:clickhouse://127.0.0.1:8123/default
ru.yandex.clickhouse.ClickHouseDriver
```

Всё работает - проверено.


#__________Рома нас спас. Датагрибом можно пользоваться теперь с локального компа

На виртуалке он сделал вот что для постгреса:
```
разрешил подключение со всех интерфейсов в файле /etc/postgresql/12/main/postgresql.conf listen_addresses = '*' 
и в файле /etc/postgresql/12/main/pg_hba.conf разрешил все сети 
host    all             all             0.0.0.0/0            md5
```

И вот что для cl:
```
ОК. Я понял - в файле /etc/clickhouse-server/config.xml строка
<listen_host>::</listen_host>

[11:30] Роман Нечаев
да только открыл 0.0.0.0
скажем так под ipv4 

Идем в файл и делаем вот так чтобы стало в строке "listen_host"
   <!-- Same for hosts without support for IPv6: -->
     <listen_host>0.0.0.0</listen_host>
```


Важно, адрес без всяких хттп. 192.168.7.165