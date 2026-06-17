# 08. Raw Store MinIO

## Цель

Реализовать abstraction для хранения неизменяемых raw snapshots в MinIO.

## Контекст

Каждый материал из источника должен сохраняться как raw snapshot, чтобы любой вывод можно было раскрыть до исходного текста.

## Работы

- Добавить MinIO client dependency.
- Создать `RawStoreService`.
- Создать конфигурацию bucket/object key prefix.
- Реализовать запись raw snapshot.
- Реализовать чтение raw snapshot.
- Реализовать content hash.
- Сохранять metadata raw snapshot в PostgreSQL.
- Добавить smoke test записи/чтения.

## Object key convention

```text
logrus-raw/
  source-code/
    yyyy/mm/dd/
      raw-snapshot-id.html
      raw-snapshot-id.xml
      raw-snapshot-id.pdf
      raw-snapshot-id.txt
```

## Metadata в БД

- `raw_snapshot_id`
- `source_id`
- `source_url`
- `content_type`
- `object_key`
- `content_hash`
- `fetched_at`
- `http_status`
- `metadata`

## Зависимости

- `02-local-infrastructure.md`
- `03-database-liquibase-foundation.md`
- `04-logrus-api-skeleton.md`

## Результат

- API может сохранить raw snapshot в MinIO.
- Ссылка на объект сохраняется в БД.
- Snapshot не перезаписывается.

## Acceptance Criteria

- Запись raw snapshot работает локально.
- Чтение raw snapshot работает локально.
- Content hash сохраняется.
- Повторный snapshot создает новую версию/запись, а не затирает старую.

## Не входит

- Lifecycle policies.
- Архивация.
- Шифрование на уровне object store.
- Production backup.
