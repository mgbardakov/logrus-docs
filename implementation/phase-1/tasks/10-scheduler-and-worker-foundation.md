# 10. Scheduler & Worker Foundation

## Цель

Заложить простой механизм фоновых задач средствами Spring Boot.

## Контекст

В MVP не используем отдельную очередь сообщений. Для первой версии достаточно `@Scheduled`, `TaskExecutor`, статусов задач в БД и аккуратной обработки ошибок.

## Работы

- Включить scheduling.
- Настроить bounded `TaskExecutor`.
- Создать модель `SourceRun`.
- Создать сервис запуска source run.
- Создать статусную модель: `PENDING`, `RUNNING`, `SUCCEEDED`, `FAILED`.
- Добавить manual trigger endpoint.
- Добавить basic retry policy на уровне кода.
- Логировать duration, errors, количество обработанных материалов.

## Минимальные endpoints

```text
POST /api/internal/sources/{sourceCode}/run
GET /api/internal/source-runs
GET /api/internal/source-runs/{id}
```

## Зависимости

- `03-database-liquibase-foundation.md`
- `04-logrus-api-skeleton.md`
- `07-topic-and-source-config.md`

## Результат

- Источник можно запустить вручную.
- Scheduler может запускать источники по расписанию.
- Статусы запусков сохраняются.

## Acceptance Criteria

- Manual trigger создает `source_run`.
- `source_run` проходит статусы.
- Ошибка задачи сохраняется и видна через API.
- Повторный запуск не блокирует приложение.

## Не входит

- Kafka/RabbitMQ.
- Distributed locking.
- Complex workflow engine.
- Real connectors P0.
