# 03. Database & Liquibase Foundation

## Цель

Настроить PostgreSQL, pgvector и Liquibase как основу хранения данных MVP.

## Контекст

Даже если часть таблиц будет использоваться позже, в фазе 1 нужно заложить базовую модель: источники, сырые снимки, документы, темы, события, связи, evidence, гипотезы и review.

## Работы

- Подключить Liquibase в `logrus-api`.
- Создать master changelog.
- Создать стартовые changelog-группы.
- Добавить расширение `vector`, если оно доступно в окружении.
- Создать минимальные таблицы.
- Добавить базовые индексы.
- Добавить audit поля: `created_at`, `updated_at`.
- Добавить enum/check constraints для ключевых типов.

## Стартовые changelog-группы

```text
000-init-schema.yaml
010-auth-users.yaml
020-sources.yaml
030-documents-raw-snapshots.yaml
040-topics.yaml
050-events.yaml
060-entities.yaml
070-relations-evidence.yaml
080-hypotheses-briefs.yaml
090-review-observability.yaml
```

## Минимальные таблицы

- `users`
- `workspaces`
- `sources`
- `source_runs`
- `raw_snapshots`
- `documents`
- `topics`
- `events`
- `entities`
- `event_entities`
- `event_topics`
- `relations`
- `relation_topic_scores`
- `evidence`
- `analytical_hypotheses`
- `briefs`
- `review_items`

## Зависимости

- `02-local-infrastructure.md`
- `04-logrus-api-skeleton.md`

## Результат

- Схема БД создается на чистой базе.
- Приложение стартует с Liquibase.
- Есть основа для следующих фаз.

## Acceptance Criteria

- Liquibase успешно применяет миграции на пустой БД.
- Повторный запуск миграций идемпотентен.
- В тестах можно поднять чистую БД и применить changelog.
- Таблицы покрывают базовую модель фазы 1.

## Не входит

- Полная оптимизация индексов.
- Финальная схема под все будущие источники.
- Сложные materialized views.
