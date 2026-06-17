# 11. Security Foundation

## Цель

Добавить базовую безопасность MVP для API, BFF и UI без enterprise-сложности.

## Контекст

На Phase 1 достаточно простой auth модели, которая позволит различать пользователя и аналитика. Enterprise SSO и сложная ролевая модель откладываются.

## Работы

- Подключить Spring Security в `logrus-api` и `logrus-bff`.
- Выбрать MVP auth mode: basic auth, form login или session-based auth через BFF.
- Создать роли `USER` и `ANALYST`.
- Защитить internal endpoints.
- Защитить review endpoints.
- Добавить local users для разработки.
- Добавить CSRF decision для BFF/UI.
- Настроить CORS/dev proxy для локального frontend.

## Роли

```text
USER:
  читает доступные/зафиксированные сводки, события, карточки

ANALYST:
  читает все пользовательское
  видит сводки на проверке
  видит review queue
  может подтверждать/править события, связи и статусы сводок
```

## Зависимости

- `04-logrus-api-skeleton.md`
- `05-logrus-bff-skeleton.md`
- `06-angular-ui-skeleton.md`

## Результат

- Базовые endpoints защищены.
- UI может работать с выбранной auth моделью.
- Роль аналитика отделена от обычного пользователя.

## Acceptance Criteria

- Неавторизованный запрос к защищенным endpoints отклоняется.
- Пользователь с ролью `USER` не видит review endpoints.
- Пользователь с ролью `USER` не видит рабочие сводки на проверке, если они не опубликованы/не зафиксированы.
- Аналитик видит review endpoints.
- Local development flow задокументирован.

## Не входит

- SSO.
- OAuth/OIDC как обязательное требование.
- Multi-tenant enterprise policy.
- Audit-grade security.
