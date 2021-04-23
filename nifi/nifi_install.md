#Настройка Apache NiFi on Linux
Меня привлекло вот такое описание. Будем делать по нему

https://nathanlabadie.com/the-basics-how/

##________Установка Maven (Это если хотите из исходников собирать. Пропускаем)
Вот тут написано что нужен чудо-мейвен. 

https://nifi.apache.org/quickstart.html

Ставим:
```
sudo apt-get install maven
```

##________Устновка JAVA. Обязательно отредактировать etc/profile
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

##________Скачиваем последнюю версию NiFi и устанавливаем

Переходим на страницу загрузки тыкаем по ссылке в разделе «Двоичные файлы»

https://nifi.apache.org/download.html

попадаем на страницу загрузки - копируем первую ссылку, у меня такая получилась:
```
wget https://apache-mirror.rbc.ru/pub/apache/nifi/1.12.1/nifi-1.12.1-bin.tar.gz
tar vzxf nifi-1.12.1-bin.tar.gz
```
Тоесть установка сводиться к скачиванию и распаковки дистрибутива. 
И его даже уже можно запустить. Но для продакшн надо установить его как сервис.

ОФДОК:
>Например, чтобы установить NiFi как сервис с именем dataflow,
>используйте команду bin/nifi.sh install dataflow.

Но предварительно надо настроить конфиг. Лучше сделать это сразу. 
Лезем в папку config там ищем файл nifi.properties 

ОФДОК:
>Внесите необходимые изменения в файлы, находящиеся в <installdir>/conf
>
>Как минимум, мы рекомендуем отредактировать файл nifi.properties 
>и ввести пароль для nifi.sensitive.props.key(см. Свойства системы ниже)
"""

Описание того что можно корректировать перед запуском:
https://nifi.apache.org/docs/nifi-docs/html/administration-guide.html#system_properties

Описание того зачем корректировать ключ шифрования: 
https://nifi.apache.org/docs/nifi-docs/html/administration-guide.html#nifi_sensitive_props_key

Вобщем открываем файл, ищем строку и делаем так (где после равно - ваш ключ):

```
nifi.sensitive.props.key=asBeer342566idijjk3224hshfn5mnul
```

Для продакшн использовать опыт AirFlow

```
python
from cryptography.fernet import Fernet
key = Fernet.generate_key()
print(key)
exit(0)

nifi.sensitive.props.key=key
```

Далее открываем /bin/nifi_env.sh и прописываем ему java_home:

https://community.cloudera.com/t5/Support-Questions/JAVA-HOME-not-set-results-may-vary-no-solution-found/m-p/303369

```
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
```

После конфигурации - устанавливаем как сервис

```
sudo ./nifi.sh install
```

Должно появиться:

```
Service nifi installed
```

Переходим в корень и запускаем вот так:

```
cd ~
sudo /etc/init.d/nifi start
```
В документации описано что можно назначить имя сервсису - лучше не надо. 
При запуске получим предупреждения какие-то, пока их можно игнорировать

НА ПРЕДУПРЕЖДЕНИЯ МОЖНО ЗАБИТЬ - ЭТО ПРОБЛЕМА JDK. 
точнее нет проблемы в NiFi - просто уведомлялки бесполезные. 

Ещё люди пишут что это 11й jdk такой неугомонный 
эта проблема связана с последними обновлениями java. 
настройте его с помощью java 8., он работает быстро.

Решение кроется в правах доступа, но там для винды:
https://community.cloudera.com/t5/Support-Questions/Nifi-Installation-error/td-p/176845

У нас адрес вот такой:
http://192.168.7.165:8080/nifi/

В консоли вбить вот это, если интернет отвалится:

```
sudo echo "nameserver 8.8.8.8" >> /etc/resolv.conf
```

И ещё кое - что... 
В одной статье увидел настройку NiFi из-под нового пользователя. 
Думаю пойдёт в продакшн 
https://yandex.ru/turbo/dataenginer.ru/s/?pcgi=p%3D5401
