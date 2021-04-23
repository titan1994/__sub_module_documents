Запуск служб

```
cd ~
sudo /etc/init.d/nifi_two start
sudo /etc/init.d/nifi-registry start
sudo /etc/init.d/clickhouse-server start
```

Чинить интернет:
```
sudo echo "nameserver 8.8.8.8" >> /etc/resolv.conf
cd ~
sudo nano /etc/resolv.conf
nameserver 8.8.8.8
ctrl+x
y

```
