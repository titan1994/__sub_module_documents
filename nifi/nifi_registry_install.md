
#________Установка и настройка NiFi Registry. Продолжение. Уходи - если не прочел README
Зачем это всё? Это контроль версий для создаваемых потоков 
А вы подумывали на гитхаб пулять? ;-)

Продолжаем веселье. Спустя полтора дня мы неожиданно узнаем что есть 
ещё один весёлый инструмент для обслуживания NiFi. 
Качается отдельно, ставится отдельно 

https://nifi.apache.org/docs/nifi-registry-docs/html/getting-started.html
https://nifi.apache.org/docs/nifi-registry-docs/html/administration-guide.html#how-to-install-and-start-nifi-registry

#________Установка и настройка NiFi Registry

Переходим по ссылке 
https://nifi.apache.org/registry.html

Ищем бинарник и распакуем рядом снайфаем 

```
wget https://apache-mirror.rbc.ru/pub/apache/nifi/nifi-registry/nifi-registry-0.8.0/nifi-registry-0.8.0-bin.tar.gz
tar vzxf nifi-registry-0.8.0-bin.tar.gz
```

Сразу уже на опыте правим JDK в конфигуарции виртуального окружения (nifi-registry-env.sh) на такой какой в найфае:
```
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
```

Ставим как услугу: 

```
sudo ./nifi-registry.sh install
```

Запускаем из корня через пень-колоду, вобщем также как и найфай сам:

```
cd ~
sudo /etc/init.d/nifi-registry start
```

http://192.168.7.165:18080/nifi-registry

#________Как версионировать NiFi объекты в NiFi Registry:

https://community.cloudera.com/t5/Community-Articles/Versioned-DataFlows-with-Apache-NiFi-1-5-and-Apache-NiFi/ta-p/247737

НЕНАВИЖУ!!! В КОНЦЕ ВСЕЙ ДОКУМЕНТАЦИИ ССЫЛКА НА ПОДРОБНЫЙ ПОШАГОВЫЙ МАНУАЛ УСТАНОВКИ!!!

https://nifi.apache.org/docs/nifi-docs/html/walkthroughs.html#securing-nifi-with-tls