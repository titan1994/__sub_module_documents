# Подмодули гитхаба
Крутая штука 

## Добавление подмодуля и инициализация
Скопировать в проект подмодуль: 
```
git submodule add https://github.com/titan1994/__sub_module_scripts MODS/scripts
git submodule init
```

## Рекурсивное получение актуальных правок по всем подмодулям 

Переходим в нужную ветку делаем пул

```
git submodule foreach --recursive git checkout master
git submodule foreach --recursive git pull
```

## Рекурсивно отправить все изменения во все подмодули из текущего проекта

Делаем коммит и пуш

```
git submodule foreach --recursive git commit -a -m "WorkFlow. IvanKozlov. Тест"
git submodule foreach --recursive git push
```

## Удалить подмодуль 

Сначала отсоединяем 
```
git submodule deinit -f -- __scripts
```

Удаляем каталог в ОС
```
rd /S ./.git/modules/__scripts - На винде не работает. Руками удаляем
rm -rf .git/modules/__scripts
```

В конце удаляем из гита
```
git rm -f __scripts
git rm --cached __scripts
```

## Что может всё испортить

Обновить код всех подмодулей до последней версии 
(ИЗ ПОДМОДУЛЕЙ В ПРОЕКТ, вводит путаницу, делаем подругому):

```
git submodule update --recursive --remote
```



