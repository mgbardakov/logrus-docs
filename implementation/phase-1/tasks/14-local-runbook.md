# 14. Local Runbook

## Цель

Подготовить инструкцию локального запуска и проверки Phase 1.

## Контекст

Фундамент MVP должен быть воспроизводимым. Новый разработчик или будущий мы должны быстро понять, как поднять БД, MinIO, API, BFF и UI.

## Работы

- Описать prerequisites.
- Описать запуск инфраструктуры.
- Описать запуск `logrus-api`.
- Описать запуск `logrus-bff`.
- Описать запуск Angular UI.
- Описать проверку health endpoints.
- Описать доступ к MinIO console.
- Описать применение Liquibase.
- Описать запуск тестов.
- Описать типовые ошибки.

## Минимальные команды

```text
docker compose up
(cd logrus-api && ./gradlew bootRun)
(cd logrus-bff && ./gradlew bootRun)
(cd logrus-bff/logrus-ui && pnpm install)
(cd logrus-bff/logrus-ui && pnpm start)
(cd logrus-bff/logrus-ui && pnpm build:static)
(cd logrus-api && ./gradlew test)
(cd logrus-bff && ./gradlew test)
```

Финальные команды будут уточнены после выбора фактической структуры проекта.

## Зависимости

- Все задачи Phase 1.

## Результат

- Есть README/runbook для локального запуска.
- Definition of Done Phase 1 можно проверить по инструкции.

## Acceptance Criteria

- Документ содержит пошаговый happy path.
- Документ содержит URLs локальных сервисов.
- Документ содержит тестовые credentials, если они нужны.
- Документ содержит troubleshooting для БД, MinIO, портов и миграций.

## Не входит

- Production runbook.
- Deployment pipeline.
- Incident response guide.
