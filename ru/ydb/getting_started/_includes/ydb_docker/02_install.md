---
sourcePath: ru/ydb/ydb-docs-core/ru/core/getting_started/_includes/ydb_docker/02_install.md
---
## Выгрузите Docker-образ {{ ydb-short-name }} {#install}

Выгрузите актуальную публичную версию Docker-образа:

```bash
docker pull {{ ydb_local_docker_image }}:{{ ydb_local_docker_image_tag }}
```

Проверьте, что Docker-образ успешно выгружен:

```bash
docker image list | grep {{ ydb_local_docker_image }}
```

Результат выполнения:

```bash
{{ ydb_local_docker_image }}           {{ ydb_local_docker_image_tag }}   b73c5c1441af   2 months ago   793MB
```
