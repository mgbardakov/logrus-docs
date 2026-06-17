# 04. logrus-api Skeleton

## Цель

Создать каркас backend core приложения `logrus-api` на Spring Boot + Kotlin + Spring MVC.

## Контекст

`logrus-api` — основное приложение доменной логики. В фазе 1 оно должно стартовать, подключаться к PostgreSQL/MinIO, отдавать health и базовые internal endpoints.

## Работы

- Создать Spring Boot приложение `logrus-api`.
- Настроить Kotlin, Spring MVC, validation, actuator.
- Подключить PostgreSQL datasource.
- Подключить Liquibase.
- Подготовить package structure.
- Добавить базовый error handling.
- Добавить OpenAPI позже или stub, если потребуется.
- Добавить `/actuator/health`.
- Добавить минимальный internal controller.

## Предлагаемые пакеты

```text
ru.logrus.api
  config
  common
  security
  source
  ingestion
  rawstore
  document
  event
  entity
  topic
  relation
  evidence
  hypothesis
  brief
  llm
  review
  observability
```

## Минимальные endpoints

```text
GET /actuator/health
GET /api/internal/status
GET /api/internal/topics
GET /api/internal/sources
```

## Зависимости

- `01-project-scaffold.md`
- `02-local-infrastructure.md`
- `03-database-liquibase-foundation.md`

## Результат

- `logrus-api` запускается локально.
- Health endpoint работает.
- Приложение готово к добавлению доменной логики.

## Acceptance Criteria

- Application context test проходит.
- `/actuator/health` возвращает `UP`.
- Приложение подключается к PostgreSQL.
- Liquibase запускается при старте.
- Ошибки API возвращаются в едином формате.

## Не входит

- Реальные source connectors.
- LLM pipeline.
- Полноценный CRUD всех сущностей.
