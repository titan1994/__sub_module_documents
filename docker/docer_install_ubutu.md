#______Первый опыт работы с докером в убунту
ВСЁ ЗАПУСКАТЬ ОТ СУДО

#______Установка

Зависимости:

```
sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
```

Установка:

```
sudo apt-get install docker docker.io
```

Разрешаем запуск сервиса docker:
```
sudo systemctl enable docker
```

и запускаем его:

```
sudo systemctl start docker
```

Проверка:

```
sudo docker run hello-world
```

Хер там, чиним: 

1. Open config file sudo nano /etc/resolv.conf and add the following under existing nameservers

```
nameserver 8.8.8.8
nameserver 8.8.4.4
```

2. run following commands to restart daemon and docker service

```
sudo systemctl daemon-reload
sudo systemctl restart docker
sudo docker run hello-world
```

ГОТОВО

##__________Первые шаги. Список команд

Список образов, список контейнеров
```
sudo docker images
sudo docker ps
```

##__________Первые шаги. Первй докерфайл

Идем в пайчарм, создаем простой хелловорлд. 

И делаем вот такой докерфайл (прямо в проекте "Dockerfile" - без расширения)

```
FROM python:3.6

RUN mkdir -p /usr/src/app/
WORKDIR /usr/src/app/
COPY . /usr/src/app/

CMD ["python", "hello.py"]
```

Открываем терминал пайчарма и собираем образ, запускаем

```
sudo docker build -t hello_world .
sudo docker run hello_world

```

Запуск контейнера с именем/Просмотр всех контейнеров в том числе и остановленных: 

```
sudo docker run --name ff  hello_world
sudo docker ps -a
```

Фишка с вложенными командами  (остановка/удаление всех контейнеров)

```
sudo docker stop $(sudo docker ps -qa)
sudo docker rm $(sudo docker ps -qa)
```

Запуск контейнера в фоне (-d) / Запуск в фоне + удаление после завершения (--rm)
```
sudo docker run --name ff -d  hello_world
sudo docker run --name f1 -d --rm  hello_world
```

Создание файла requirements 

```
sudo pipenv lock -r > requirements.txt
```

Передача порта и переменных окружения

```
sudo docker run --name ff -d -p 8080:8080 -e TZ=Europe/Moscow hello_world
```

##_____Docker compose

```
https://www.digitalocean.com/community/tutorials/how-to-install-docker-compose-on-ubuntu-18-04-ru
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```
 
##______Docker hub 

```
docker build -t atom1994/test_flask .
docker push  atom1994/test_flask
```

##______Dockerfile
https://www.dmosk.ru/miniinstruktions.php?mini=docker-self-image

