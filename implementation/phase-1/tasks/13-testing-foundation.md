# 13. Testing Foundation

## Цель

Создать минимальный тестовый фундамент, который защитит каркас MVP от поломок при следующих фазах.

## Контекст

На первой фазе тесты должны проверять запуск приложений, миграции, подключение к БД, MinIO и базовые API.

## Работы

- Настроить test dependencies.
- Добавить focused integration/smoke tests для `logrus-api`.
- Добавить focused integration/smoke tests для `logrus-bff`.
- Добавить Testcontainers для PostgreSQL.
- Добавить тест Liquibase migrations.
- Добавить smoke test MinIO raw store.
- Добавить MVC tests для health/status endpoints.
- Добавить BFF client contract smoke test.
- Добавить Angular basic test setup.

## Минимальные тесты

```text
logrus-api:
  liquibase applies on clean db
  health endpoint works
  raw store write/read works
  topic config loads
  brief list endpoint returns stub/list model

logrus-bff:
  health endpoint works
  dashboard endpoint returns panel metrics
  briefs endpoint returns list
  packaged UI routes forward to Angular index

frontend:
  app renders
  dashboard/panel component renders
  briefs page renders
```

## Зависимости

- `03-database-liquibase-foundation.md`
- `04-logrus-api-skeleton.md`
- `05-logrus-bff-skeleton.md`
- `06-angular-ui-skeleton.md`
- `08-raw-store-minio.md`

## Результат

- Есть быстрый набор smoke/integration tests.
- Следующие фазы могут менять код без страха сломать фундамент незаметно.

## Acceptance Criteria

- Backend tests проходят локально.
- Liquibase test проходит на чистой БД.
- Raw store test пишет и читает объект.
- BFF test проверяет dashboard endpoint и список сводок.
- Frontend basic tests проходят.

## Не входит

- Полный e2e test suite.
- Load testing.
- Eval тесты LLM.
- Контрактное тестирование всех API.
