# 12. Observability Foundation

## Цель

Добавить базовые health checks, metrics и структурированные логи для MVP.

## Контекст

Даже на MVP нужно видеть, что приложение живо, источники запускаются, ошибки не теряются, а стоимость/количество LLM-вызовов в будущем можно будет измерять.

## Работы

- Подключить Spring Boot Actuator.
- Включить health endpoints.
- Настроить Micrometer metrics.
- Добавить логирование source runs.
- Добавить correlation/request id.
- Добавить базовый error logging.
- Добавить метрики:
  - количество source runs;
  - длительность source runs;
  - количество raw snapshots;
  - ошибки raw store;
  - количество событий;
  - количество кандидатных связей событие-событие;
  - количество подтвержденных/отклоненных связей;
  - распределение confidence по связям;
  - будущие LLM calls/cost placeholders.

## Минимальные endpoints

```text
GET /actuator/health
GET /actuator/metrics
```

## Зависимости

- `04-logrus-api-skeleton.md`
- `05-logrus-bff-skeleton.md`
- `10-scheduler-and-worker-foundation.md`

## Результат

- Health и метрики доступны локально.
- Ошибки фоновых задач видны в логах и БД.

## Acceptance Criteria

- Health endpoint показывает состояние БД.
- Health endpoint показывает состояние MinIO или raw store service.
- Source run errors логируются.
- Метрики можно открыть через actuator.
- Есть placeholders метрик для будущего relation scoring и review решений.

## Не входит

- Prometheus/Grafana как обязательная инфраструктура.
- Centralized logging.
- Alertmanager.
- Production incident process.
