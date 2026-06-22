# ЛОГРУС MVP: задачи фазы 1

Фаза: технический фундамент  
Дата фиксации: 2026-06-17  
Связанный документ: [logrus-phase-1-technical-foundation.md](../logrus-phase-1-technical-foundation.md)

Цель фазы: создать рабочий каркас MVP, на котором можно реализовать сбор источников, хранение сырых материалов, нормализацию, события, связи, сводки и пользовательский интерфейс.

Правило доменной модели фазы: прямые факты события хранятся через специализированные таблицы (`event_entities`, `event_topics`, документы и raw snapshots), а `relations` используются только для связей событие-событие. Временной порядок является признаком связи, а не самостоятельным типом связи.

Целевая цепочка фазы:

```text
Angular UI -> Spring Boot Kotlin BFF -> Spring Boot Kotlin API -> PostgreSQL / MinIO
```

## Задачи

1. [01-project-scaffold.md](01-project-scaffold.md) — структура проекта и build setup.
2. [02-local-infrastructure.md](02-local-infrastructure.md) — локальная инфраструктура Docker Compose.
3. [03-database-liquibase-foundation.md](03-database-liquibase-foundation.md) — PostgreSQL, pgvector и Liquibase.
4. [04-logrus-api-skeleton.md](04-logrus-api-skeleton.md) — каркас `logrus-api`.
5. [05-logrus-bff-skeleton.md](05-logrus-bff-skeleton.md) — каркас `logrus-bff`.
6. [06-angular-ui-skeleton.md](06-angular-ui-skeleton.md) — каркас Angular frontend.
7. [07-topic-and-source-config.md](07-topic-and-source-config.md) — конфигурация темы MVP и источников.
8. [08-raw-store-minio.md](08-raw-store-minio.md) — raw store поверх MinIO.
9. [09-domain-stubs-and-api.md](09-domain-stubs-and-api.md) — минимальные доменные сущности и stub API.
10. [10-scheduler-and-worker-foundation.md](10-scheduler-and-worker-foundation.md) — scheduler и worker-фундамент.
11. [11-security-foundation.md](11-security-foundation.md) — базовая безопасность MVP.
12. [12-observability-foundation.md](12-observability-foundation.md) — health, metrics, logging.
13. [13-testing-foundation.md](13-testing-foundation.md) — тестовый фундамент.
14. [14-local-runbook.md](14-local-runbook.md) — инструкция локального запуска.

## Definition of Done фазы

- `logrus-api` и `logrus-bff` запускаются локально.
- Angular UI открывается через BFF или dev server.
- PostgreSQL и MinIO поднимаются через Docker Compose.
- Liquibase создает стартовую схему.
- Конфигурация темы MVP загружается приложением.
- Можно создать/прочитать источник, документ-заглушку, событие-заглушку и список аналитических сводок.
- Карточка события различает прямые факты события и связанные события.
- Raw snapshot можно записать в MinIO и сохранить ссылку в БД.
- Health endpoints работают.
- Есть базовые логи и метрики.
- Документация содержит инструкции локального запуска.
