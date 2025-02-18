# Типы хранилища

{{ mgp-name }} позволяет использовать сетевые и локальные диски для организации хранилища кластеров баз данных. Сетевые диски реализованы на базе сетевых блоков — виртуальных дисков в инфраструктуре {{ yandex-cloud }}. Локальные диски физически размещаются в серверах кластера.

{% include [storage-type](../../_includes/mdb/mgp/storage-type.md) %}

В кластере {{ mgp-name }} тип хранилища у хостов-мастеров и хостов-сегментов может отличаться.

{% include [local-ssd для Ice Lake только по запросу](../../_includes/ice-lake-local-ssd-note.md) %}

## Особенности хранилища на локальных SSD-дисках {#local-storage-features}

Хранилище на локальных SSD-дисках не обеспечивает отказоустойчивости хранения данных, а также влияет на тарификацию кластера в целом:

* Такое хранилище в кластере из одного хоста не обеспечивает отказоустойчивости: при отказе диска данные теряются безвозвратно. Поэтому при создании нового кластера {{ mgp-name }} с использованием этого типа хранилища автоматически настраивается отказоустойчивая конфигурация c двумя хостами-мастерами.
* Кластер с таким хранилищем тарифицируется, даже если он остановлен. Подробнее — в [правилах тарификации](../pricing).

## Особенности хранилища на нереплицируемых SSD-дисках {#network-nrd-storage-features}

Одиночный хост-мастер с хранилищем на нереплицируемых SSD-дисках не отказоустойчив: при отказе диска данные теряются безвозвратно. Поэтому при использовании этого типа хранилища можно выбрать только конфигурацию с двумя хостами-мастерами.

{% include [greenplum-trademark](../../_includes/mdb/mgp/trademark.md) %}
