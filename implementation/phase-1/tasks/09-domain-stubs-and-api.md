# 09. Domain Stubs & API

## Цель

Создать минимальные доменные сущности и API-заглушки, чтобы UI и BFF могли работать до реализации реального пайплайна.

## Контекст

Фаза 1 должна дать end-to-end skeleton: пользователь открывает UI, BFF запрашивает API, API возвращает события/источники/темы/аналитические сводки из БД или stub данных.

Аналитическая сводка — не единственный объект домашней страницы. Сводок может быть несколько, они накапливаются в статусе “На проверке” и отображаются списком на панели аналитика и на отдельном экране `/briefs`.

## Работы

- Создать модели `Source`, `Document`, `Event`, `Topic`.
- Создать модель `Brief` / `AnalyticalBrief` в минимальном виде.
- Создать модели `Relation`, `Evidence`, `AnalyticalHypothesis` в минимальном виде.
- Создать repository/service/controller слои для чтения.
- Добавить seed/test data для local profile.
- Добавить endpoints для списка событий и карточки события.
- Добавить endpoints для списка аналитических сводок.
- Добавить view models для BFF.

## Минимальные поля события

- `id`
- `type`
- `title`
- `summary`
- `publishedAt`
- `sourceId`
- `sourceUrl`
- `topicCodes`
- `confidence`

## Минимальные поля связи

- `id`
- `sourceEventId`
- `targetEventId`
- `relationLevel`: `FACTUAL`, `THEMATIC`, `HYPOTHESIS`
- `relationType`
- `confidence`
- `explanation`

## Минимальные поля аналитической сводки

- `id`
- `title`
- `summary`
- `periodLabel` или `periodStart`/`periodEnd`
- `status`: `PREPARING`, `PENDING_REVIEW`, `NEEDS_CHANGES`, `FINALIZED`, `ARCHIVED`
- `createdAt`
- `updatedAt`
- `eventCount`

В MVP создание сводок инициируется системой/worker после накопления новых значимых событий. Ручное создание сводки аналитиком можно добавить позже, но оно не является обязательным для Phase 1.

## Зависимости

- `03-database-liquibase-foundation.md`
- `04-logrus-api-skeleton.md`
- `05-logrus-bff-skeleton.md`
- `06-angular-ui-skeleton.md`

## Результат

- UI может отображать панель аналитика, список сводок на проверке, события и карточку события.
- API содержит минимальный доменный слой.

## Acceptance Criteria

- `GET /api/internal/events` возвращает список событий.
- `GET /api/internal/events/{id}` возвращает карточку события.
- `GET /api/internal/briefs` возвращает список аналитических сводок.
- `GET /bff/briefs` возвращает UI-ready список сводок.
- `GET /bff/events/{id}` возвращает UI-ready модель.
- В карточке виден тип связи: фактическая, тематическая или гипотеза.

## Не входит

- Реальное извлечение событий.
- Реальный relation scoring.
- Полный CRUD.
