---
sourcePath: ru/ydb/ydb-docs-core/ru/core/getting_started/_includes/ydb_docker/03_start.md
---
## Запустите Docker-контейнер YDB {#start}

Запустите {{ ydb-short-name }} Docker-контейнер и смонтируйте нужные директории:

```bash
docker run -d \
  --rm \
  --name ydb-local \
  -h localhost \
  -p 2135:2135 \
  -p 8765:8765 \
  -p 2136:2136 \
  -v $(pwd)/ydb_certs:/ydb_certs -v $(pwd)/ydb_data:/ydb_data \
  -e YDB_DEFAULT_LOG_LEVEL=NOTICE \
  -e GRPC_TLS_PORT=2135 \
  -e GRPC_PORT=2136 \
  -e MON_PORT=8765 \
  {{ ydb_local_docker_image}}:{{ ydb_local_docker_image_tag }}
```

Где:

* `-d` — запустить Docker-контейнер в фоновом режиме.
* `--rm` — удалить контейнер после завершения его работы.
* `--name` — имя контейнера.
* `-h` — имя хоста контейнера.
* `-p` — опубликовать порты контейнера на хост-системе.
* `-v` — монтировать директории хост-системы в контейнер.
* `-e` — задать переменные окружения.

{% note info %}

Инициализация Docker-контейнера, в зависимости от мощности хост-системы, может занять несколько минут. До окончания инициализации база данных будет недоступна.

{% endnote %}

Docker-контейнер {{ ydb-short-name }} поддерживает дополнительные опции, которые можно задать через переменные окружения:

* `YDB_USE_IN_MEMORY_PDISKS=true` — включает возможность хранения данных целиком в памяти. В случае если данная опция включена, рестарт контейнера с локальной {{ ydb-short-name }} приведет к полной потере данных.
* `YDB_DEFAULT_LOG_LEVEL=<уровень>` — задает уровень логирования. Доступные значения уровней: `CRIT`, `ERROR`, `WARN`, `NOTICE`, `INFO`.
* `YDB_PDISK_SIZE=<NUM>GB` - задает размер диска для хранения данных базы данных.
* `GRPC_PORT` - порт для взаимоидествия с {{ ydb-short-name }} API про gRPC без TLS.
* `GRPC_TLS_PORT` - порт для взаимоидествия с {{ ydb-short-name }} API про gRPC c поддержкой TLS.
* `MON_PORT` - порт сo встроенными средствами мониторинга и интроспекции.
