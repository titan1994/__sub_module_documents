#Запуск контейнера постгреса с внешним томом на винде

##--------Решение проблемы с виндой 

Первое с чем мы сталкиваемся - это проблема. 
С тем что всё не для винды... 

https://github.com/docker/for-win/issues/445

Человек решил проблему, создав новый том вроде этого:

```
docker volume create postgres_database
```

А потом вы связываете это так:

```
services:
    postgres:
        restart: always
        volumes:
            - postgres_database:/var/lib/postgresql/data

volumes:
    postgres_database:
        external: true

```
Тоесть сначала мы создаем в терминале внешний волюм, а потом прикручиваем его к постгресу
Возможно с другими образами тоже самое надо делать когда ошибка "permission denied" 


Модернизация - добавили свой путь (Не для винды... )
```
docker volume create --name psql_data --opt type=none --opt device=C:\Users\i.kozlov\PycharmProjects\ISCSAPK_BI\postgres\

docker volume create --name psql_db --driver local --opt type=none --opt device=/var/lib/postgresql/data --opt o=bind
```