# 05. logrus-bff Skeleton

## Цель

Создать BFF-приложение на Spring Boot + Kotlin + Spring MVC для обслуживания Angular UI.

## Контекст

BFF нужен, чтобы frontend не зависел от внутренних API-контрактов `logrus-api`. На MVP он агрегирует данные для экранов сводки, карточки события и review.

## Работы

- Создать Spring Boot приложение `logrus-bff`.
- Настроить Spring MVC, validation, actuator.
- Создать client layer для вызовов `logrus-api`.
- Добавить конфигурацию base URL `logrus-api`.
- Добавить BFF endpoints для dashboard и event card.
- Подготовить возможность сервировать Angular static bundle.
- Добавить базовый error handling.

## Предлагаемые пакеты

```text
ru.logrus.bff
  config
  security
  client
  dashboard
  event
  brief
  review
  user
```

## Минимальные endpoints

```text
GET /actuator/health
GET /bff/dashboard
GET /bff/events
GET /bff/events/{id}
GET /bff/briefs/latest
GET /bff/review/items
```

## Зависимости

- `01-project-scaffold.md`
- `04-logrus-api-skeleton.md`

## Результат

- BFF запускается локально.
- BFF может обратиться к `logrus-api`.
- Angular UI получает данные через BFF.

## Acceptance Criteria

- Application context test проходит.
- `/actuator/health` возвращает `UP`.
- `/bff/dashboard` возвращает stub view model.
- Ошибки вызова `logrus-api` обрабатываются предсказуемо.

## Не входит

- Полная user/session модель.
- Enterprise auth.
- Сложная агрегация данных.
