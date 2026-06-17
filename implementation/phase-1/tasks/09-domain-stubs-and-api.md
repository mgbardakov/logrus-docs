# 09. Domain Stubs & API

## Цель

Создать минимальные доменные сущности и API-заглушки, чтобы UI и BFF могли работать до реализации реального пайплайна.

## Контекст

Фаза 1 должна дать end-to-end skeleton: пользователь открывает UI, BFF запрашивает API, API возвращает события/источники/темы из БД или stub данных.

## Работы

- Создать модели `Source`, `Document`, `Event`, `Topic`.
- Создать модели `Relation`, `Evidence`, `AnalyticalHypothesis` в минимальном виде.
- Создать repository/service/controller слои для чтения.
- Добавить seed/test data для local profile.
- Добавить endpoints для списка событий и карточки события.
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

## Зависимости

- `03-database-liquibase-foundation.md`
- `04-logrus-api-skeleton.md`
- `05-logrus-bff-skeleton.md`
- `06-angular-ui-skeleton.md`

## Результат

- UI может отображать события и карточку события.
- API содержит минимальный доменный слой.

## Acceptance Criteria

- `GET /api/internal/events` возвращает список событий.
- `GET /api/internal/events/{id}` возвращает карточку события.
- `GET /bff/events/{id}` возвращает UI-ready модель.
- В карточке виден тип связи: фактическая, тематическая или гипотеза.

## Не входит

- Реальное извлечение событий.
- Реальный relation scoring.
- Полный CRUD.
