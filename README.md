# cluster-mysql


## Сборка

Из оригинального репозитория mysync собираем base image (tests/images/base):
```bash
docker build -t mysql_mysync_base:test --build-arg MYSQL_VERSION=8.0 .
```

В Dockerfile в папке tests/images/mysql делаем сборку mysync, собираем образ с БД:
```bash
docker build -t mysql_mysync_base:mysql --build-arg MYSQL_VERSION=8.0 .
```

# Запуск

Старутем docker compose, в нем будет 2 ZooKeeper (zk1 и zk2), 2 инстанса с mysync + БД (mysql1 и mysql2), ZooNavigator

ZooNavigator нужен для отображения записей в ZK, слушает на http://127.0.0.1:9080/ , строка подключения `zk1:2181,zk2:2181`.

После запуска compose на выполняем добавление нод (можно делать на каждом узле, то есть узел сам себя добавляет:
```
mysync hosts add mysql1
mysync hosts add mysql2
```

Логи:
```
cat /var/log/mysync.log
```

Статус кластера:
```
mysync info -s
```

Переключить мастера:
```
mysync switch --to mysql2
```