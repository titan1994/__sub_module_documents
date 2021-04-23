#Запуск скрипта из виртуального окружения. Вся мощь питона коннект к NiFi ;-)

##_____Запуск через pipenv-shebang
Пример запуска скрипта из окружения, который не сработает в найфае:
```
cd ~ && cd /home/uadmin/NiFi/python_script && pipenv run python metadata_postgres_to_cl.py
```

Поэтому:

https://pypi.org/project/pipenv-shebang/

```
sudo pip3 install pipenv-shebang

pipenv-shebang /home/uadmin/NiFi/python_script/metadata_postgres_to_cl.py
```

При настроёке процессора  ExecuteStreamCommand 

```
Command Arguments: /home/uadmin/NiFi/python_script/metadata_postgres_to_cl.py
Command Path: pipenv-shebang 
```

И он работает - он живой.  
Причём можно запустить какой-нибудь сервак и идти гулять. 
1000 000 фласков по расписанию ;-) 

Вцелом - читаем stdin плюём принты в stdout. 
Так и общаемся. Удобнее всего вкидывать выкидывать json. 
Так как его потом просто обернуть в атрибуты (я сам пока не знаю как, 
но есть процессор EvaluateJsonPath) 


##_____JOLT 
Это язык-библиотека трансформации json 

Вот так просто можно поток json разделить на то что нам надо (metadata) + настройки (settings)
```
[{
    "operation": "shift",
    "spec": {
      "@": "metadata"
    }
},
  {
    "operation": "default",
    "spec": {
      "settings": {"s1":1, "s2":2}
    }
}]
```