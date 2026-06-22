# ЛОГРУС MVP: Фаза 1 — технический фундамент

Дата фиксации: 2026-06-17  
Статус: high-level technical design для последующей детализации и реализации.

## 1. Цель фазы

Поднять минимальный технический фундамент MVP, на котором можно реализовать сбор источников, хранение сырых материалов, нормализацию, события, связи, сводки и пользовательский интерфейс.

Фаза 1 не должна решать всю аналитику. Ее задача — создать рабочий каркас приложения:

```text
Angular UI -> Spring Boot Kotlin BFF -> Spring Boot Kotlin API -> PostgreSQL / MinIO
```

## 2. Выбранный стек

### Backend API

- Kotlin;
- Spring Boot;
- Spring MVC;
- Spring Data JPA/JDBC;
- PostgreSQL;
- pgvector;
- Liquibase;
- MinIO для raw store.

### Frontend

- Angular;
- TypeScript;
- Angular Router;
- Angular HttpClient;
- UI-компоненты на базе выбранной Angular component library будут определены позже.

### BFF

- Spring Boot + Kotlin;
- Spring MVC;
- отдает UI-ориентированные endpoints;
- агрегирует данные из backend API;
- скрывает внутренние API-контракты от frontend;
- в MVP может также сервировать собранный Angular bundle.

### Инфраструктура MVP

- PostgreSQL — основное хранилище данных;
- pgvector — векторные представления событий и фрагментов текста;
- MinIO — хранилище сырых HTML/XML/PDF/text snapshots;
- Liquibase — управление схемой БД;
- Docker Compose — локальный запуск инфраструктуры;
- Spring Boot Actuator / Micrometer — health, metrics, базовая наблюдаемость.

## 3. Архитектурный стиль

Для MVP выбираем простую схему из двух backend-приложений в одном рабочем workspace:

1. **logrus-api** — backend core.
2. **logrus-bff** — BFF для frontend.

Важно: `logrus-api` и `logrus-bff` живут в одном верхнеуровневом проекте/workspace для разработки и локального запуска, но являются **разными Git-репозиториями**. Это не Gradle multi-module repository.

Frontend Angular живет как отдельное приложение внутри репозитория `logrus-bff` в директории `logrus-ui` и интегрируется через BFF. Для packaged MVP BFF сервирует собранный Angular bundle из Spring Boot static resources.

Причина выбора:

- меньше инфраструктурной сложности;
- проще локальная разработка;
- проще деплой первого MVP;
- можно быстро менять модель данных и API;
- при росте нагрузки отдельные компоненты можно вынести позже.

Репозиторная модель:

```text
logrus-workspace/
  logrus-api/     # git repo: backend core
  logrus-bff/     # git repo: BFF
    logrus-ui/    # Angular frontend внутри BFF repo
    src/main/resources/static/ # собранная статика Angular
  logrus-infra/   # placement TBD: отдельный repo или локальная infra-директория
```

Правило: у `logrus-api` и `logrus-bff` отдельные Gradle builds, отдельные release cycles и отдельная история Git. Общий код между ними не выносится в shared module на Phase 1; контракты фиксируются через HTTP API, DTO/view models и, позже, OpenAPI.

Не используем в первой фазе:

- микросервисы;
- Kafka/RabbitMQ;
- отдельную graph database;
- отдельный workflow engine;
- отдельный search cluster;
- Kubernetes как обязательное требование;
- отдельный auth provider, если для MVP достаточно локальной аутентификации.

## 4. Границы приложений

### logrus-api

Отвечает за:

- доменную модель;
- источники;
- документы и raw snapshots;
- события;
- сущности;
- темы;
- связи;
- evidence;
- гипотезы;
- сводки;
- фоновые задачи сбора и обработки;
- интеграцию с PostgreSQL и MinIO;
- интеграцию с LLM provider через gateway;
- внутренние API для BFF и админских операций.

### logrus-bff

Отвечает за:

- endpoints под конкретные экраны Angular;
- агрегацию данных для сводки;
- агрегацию карточки события;
- подготовку view models;
- пользовательские сессии;
- базовую авторизацию UI;
- проксирование/изоляцию внутренних API;
- serving Angular static assets.

### Angular frontend

Отвечает за:

- экран сводки;
- список событий;
- карточку события;
- отображение связей и evidence;
- фильтр по теме/подтеме;
- минимальный admin/review интерфейс для аналитика.

## 5. Использование встроенного функционала Spring Boot

В MVP дополнительные инфраструктурные компоненты не вводим, если задачу можно надежно решить средствами Spring Boot.

| Задача | Решение MVP |
|---|---|
| Периодический запуск сборщиков | `@Scheduled` |
| Фоновые задачи | `TaskExecutor`, `@Async`, bounded thread pools |
| Конфигурация тем и источников | `@ConfigurationProperties` + YAML/DB later |
| REST API | Spring MVC controllers |
| Валидация входных данных | Bean Validation |
| Транзакции | Spring `@Transactional` |
| Доступ к БД | Spring Data JPA/JDBC |
| Миграции | Liquibase |
| Health checks | Spring Boot Actuator |
| Метрики | Micrometer через Actuator |
| HTTP-клиенты к источникам | Spring `RestClient` или `WebClient` без реактивной архитектуры приложения |
| Безопасность MVP | Spring Security |
| Работа с файлами raw store | MinIO client, обернутый в Spring service |
| Ошибки и повторные попытки | Простые retry-политики в коде + статус задач в БД |

Если retry/очереди станут сложными, это будет отдельное решение после MVP, а не часть фазы 1.

## 6. Минимальная инфраструктурная схема

```text
developer machine / MVP environment

  Angular app
      |
      v
  logrus-bff
      |
      v
  logrus-api
      |             |
      v             v
  PostgreSQL     MinIO
  + pgvector     raw snapshots
```

Внешние LLM API подключаются через `LLMGateway` внутри `logrus-api`, но в фазе 1 достаточно подготовить интерфейс и mock/stub.

## 7. Минимальная модель модулей logrus-api

Предлагаемая пакетная структура:

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

Назначение:

- `source` — описание источников и их настройки;
- `ingestion` — сборщики, расписания, статусы задач;
- `rawstore` — MinIO abstraction;
- `document` — нормализованные материалы;
- `event` — события;
- `entity` — акторы, ведомства, регионы, организации;
- `topic` — конфигурации тем;
- `relation` — связи событие-событие, с evidence и тематическим скорингом;
- `evidence` — основания связей и выводов;
- `hypothesis` — аналитические гипотезы;
- `brief` — сводки;
- `llm` — gateway, prompts, model routing;
- `review` — ручная проверка;
- `observability` — технические метрики и статусы.

## 8. Минимальная модель модулей logrus-bff

Предлагаемая пакетная структура:

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

Назначение:

- `client` — клиент к `logrus-api`;
- `dashboard` — агрегированные данные главного экрана;
- `event` — карточка события и связанные события;
- `brief` — ежедневные/недельные сводки;
- `review` — UI endpoints для аналитика;
- `user` — пользовательские настройки MVP.

## 9. Данные и миграции

Liquibase должен управлять всей схемой PostgreSQL.

Стартовые changelog-группы:

```text
db/changelog
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

Минимальные таблицы фазы 1:

- `users`;
- `workspaces`;
- `sources`;
- `source_runs`;
- `raw_snapshots`;
- `documents`;
- `topics`;
- `events`;
- `entities`;
- `event_entities`;
- `event_topics`;
- `relations`;
- `relation_topic_scores`;
- `evidence`;
- `analytical_hypotheses`;
- `briefs`;
- `review_items`.

В стартовой модели `events`, `entities` и `topics` образуют один логический граф знаний, но прямые ребра ground graph хранятся отдельными таблицами: `event_entities`, `event_topics`, `documents`, `raw_snapshots`. Таблица `relations` в Phase 1 резервируется только под связи событие-событие. Временной порядок, общий актор или общее ведомство являются признаками и evidence для связи, но не самостоятельной причиной записывать все события в одну цепочку.

Для фазы 1 допускается создать часть таблиц заранее, даже если бизнес-логика появится в фазах 2-5. Это снизит миграционную суету при детализации пайплайна.

## 10. Raw store в MinIO

MinIO хранит неизменяемые снимки исходных материалов.

Базовые bucket/prefix:

```text
logrus-raw/
  source-code/
    yyyy/mm/dd/
      raw-snapshot-id.html
      raw-snapshot-id.xml
      raw-snapshot-id.pdf
      raw-snapshot-id.txt
```

В PostgreSQL хранится:

- `raw_snapshot_id`;
- `source_id`;
- `source_url`;
- `content_type`;
- `object_key`;
- `content_hash`;
- `fetched_at`;
- `http_status`;
- `metadata`.

Правило: raw snapshot не перезаписывается. Если источник изменился, создается новый snapshot/version.

## 11. Конфигурация темы MVP

В фазе 1 тема может быть YAML-конфигом, загружаемым через `@ConfigurationProperties`.

Пример:

```yaml
logrus:
  topics:
    - code: mobilization_business_risk
      title: "Мобилизационные и военно-экономические риски"
      enabled: true
      sourceCodes:
        - publication_pravo
        - regulation_gov
        - duma_api
        - government
        - kremlin
        - mintrud
        - trudvsem
      keywords:
        - мобилизация
        - мобилизационная подготовка
        - воинский учет
        - бронирование
        - отсрочка
        - повестка
        - оборонный заказ
        - ОПК
        - гражданская оборона
```

Позже эту конфигурацию можно перенести в БД и админку.

## 12. API первого фундамента

Минимальные backend endpoints `logrus-api`:

```text
GET /actuator/health
GET /api/internal/sources
POST /api/internal/sources/{sourceCode}/run
GET /api/internal/source-runs
GET /api/internal/documents
GET /api/internal/events
GET /api/internal/events/{id}
GET /api/internal/topics
```

Минимальные BFF endpoints:

```text
GET /bff/dashboard
GET /bff/events
GET /bff/events/{id}
GET /bff/briefs
GET /bff/review/items
```

На фазе 1 endpoints могут возвращать mock/stub данные там, где пайплайн еще не реализован.

## 13. Локальный запуск

Минимальный `docker-compose` должен поднимать:

- PostgreSQL с pgvector;
- MinIO;
- logrus-api;
- logrus-bff;
- Angular dev server для разработки или собранный static bundle через BFF.

Команды будут уточнены при реализации, но целевое состояние:

```text
docker compose up
```

После запуска:

- `logrus-api` отвечает на health;
- `logrus-bff` отвечает на health;
- UI открывается в браузере;
- Liquibase применяет миграции;
- MinIO доступен для записи raw snapshot.

## 14. Тестирование фазы 1

Минимальные проверки:

- focused smoke/integration tests для `logrus-api`;
- focused smoke/integration tests для `logrus-bff`;
- тест Liquibase migrations на чистой БД;
- интеграционный тест PostgreSQL через Testcontainers;
- интеграционный тест MinIO через Testcontainers или локальный container;
- smoke test для health endpoints;
- smoke test записи raw snapshot;
- smoke test чтения конфигурации темы.

## 15. Definition of Done фазы 1

Фаза считается завершенной, если:

- backend API и BFF запускаются локально;
- Angular UI открывается через BFF или dev server;
- PostgreSQL и MinIO поднимаются через Docker Compose;
- Liquibase создает стартовую схему;
- конфигурация темы MVP загружается приложением;
- можно создать/прочитать источник, документ-заглушку и событие-заглушку;
- raw snapshot можно записать в MinIO и сохранить ссылку в БД;
- health endpoints работают;
- есть базовые логи и метрики;
- документация содержит инструкции локального запуска.

## 16. Что переносится в следующие фазы

Не входит в фазу 1:

- полноценные source connectors;
- production-grade parsing HTML/PDF/XML;
- LLM extraction;
- построение реальных связей;
- генерация сводок;
- полноценный review workflow;
- сложная ролевая модель;
- enterprise auth/SSO;
- пользовательские алерты;
- деплой в production.

## 17. Связанные артефакты

- Общий план реализации: [logrus-mvp-implementation-plan.md](../logrus-mvp-implementation-plan.md)
- Дизайн-документ: [logrus-design-document.md](../../product/logrus-design-document.md)
- Задачи фазы 1: [tasks/README.md](tasks/README.md)
- Источники данных MVP: [logrus-mvp-data-sources.md](../../data/logrus-mvp-data-sources.md)
- Подробная архитектурная схема: [logrus-mvp-architecture.puml](../../architecture/logrus-mvp-architecture.puml)
- Абстрактная архитектурная схема: [logrus-mvp-architecture-high-level.puml](../../architecture/logrus-mvp-architecture-high-level.puml)
