# 02. Local Infrastructure

## Цель

Поднять локальную инфраструктуру MVP через Docker Compose: PostgreSQL с pgvector и MinIO.

## Контекст

Фаза 1 должна запускаться локально без ручной установки БД и object storage. Остальные компоненты можно запускать как из IDE, так и через Docker Compose.

## Работы

- Создать `infra/docker-compose.yml`.
- Добавить PostgreSQL с поддержкой pgvector.
- Добавить MinIO.
- Добавить initialization для bucket raw store, если это удобно.
- Настроить переменные окружения для `logrus-api` и `logrus-bff`.
- Добавить health checks контейнеров.
- Описать local ports.

## Минимальный состав

```text
postgres:
  port: 5432
  database: logrus

minio:
  api port: 9000
  console port: 9001
  bucket: logrus-raw
```

## Зависимости

- `01-project-scaffold.md`

## Результат

- Локальная инфраструктура поднимается одной командой.
- `logrus-api` может подключиться к PostgreSQL и MinIO.

## Acceptance Criteria

- `docker compose up` поднимает PostgreSQL и MinIO.
- PostgreSQL принимает подключения.
- Расширение `pgvector` доступно или может быть создано миграцией.
- MinIO console доступна локально.
- Bucket `logrus-raw` создан вручную или автоматически.

## Не входит

- Kubernetes.
- Production-grade секреты.
- Backup/restore.
- Managed cloud infrastructure.
