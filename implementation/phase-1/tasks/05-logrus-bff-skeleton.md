# 05. logrus-bff Skeleton

## Цель

Создать BFF-приложение на Spring Boot + Kotlin + Spring MVC для обслуживания Angular UI.

## Контекст

BFF нужен, чтобы frontend не зависел от внутренних API-контрактов `logrus-api`. На MVP он агрегирует данные для панели аналитика, списка аналитических сводок, карточки события и review.

Angular UI находится в этом же репозитории, в директории `logrus-ui`. В локальной разработке UI может запускаться через Angular dev server, а для packaged MVP BFF сервирует собранную статику из `src/main/resources/static`.

## Работы

- Создать Spring Boot приложение `logrus-bff`.
- Настроить Spring MVC, validation, actuator.
- Создать client layer для вызовов `logrus-api`.
- Добавить конфигурацию base URL `logrus-api`.
- Добавить BFF endpoints для dashboard, списка сводок и event card.
- Подготовить возможность сервировать Angular static bundle из `src/main/resources/static`.
- Зафиксировать placement Angular UI: `logrus-bff/logrus-ui`.
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
GET /bff/briefs
GET /bff/review/items
```

## Зависимости

- `01-project-scaffold.md`
- `04-logrus-api-skeleton.md`

## Результат

- BFF запускается локально.
- BFF может обратиться к `logrus-api`.
- Angular UI получает данные через BFF.
- BFF может отдавать статический frontend bundle.

## Acceptance Criteria

- Focused integration/smoke tests проходят без пустого `contextLoads`.
- `/actuator/health` возвращает `UP`.
- `/bff/dashboard` возвращает stub view model панели аналитика.
- `/bff/briefs` возвращает список stub-сводок.
- Ошибки вызова `logrus-api` обрабатываются предсказуемо.
- В репозитории есть директория `logrus-ui` для Angular приложения.
- Есть placeholder или подготовленная директория `src/main/resources/static` для раздачи собранной статики.

## Не входит

- Полная user/session модель.
- Enterprise auth.
- Сложная агрегация данных.
