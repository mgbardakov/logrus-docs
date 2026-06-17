# 01. Project Scaffold

## Цель

Создать базовую структуру рабочего workspace для MVP с отдельными backend-приложениями `logrus-api`, `logrus-bff` и Angular frontend внутри `logrus-bff`.

## Контекст

Фаза 1 предполагает один верхнеуровневый workspace для разработки и локального запуска, но `logrus-api` и `logrus-bff` должны быть разными Git-репозиториями. Это не Gradle multi-module repository.

Angular frontend на Phase 1 живет внутри репозитория `logrus-bff` в директории `logrus-ui`. BFF отвечает за раздачу собранного Angular static bundle из Spring Boot static resources.

На этом этапе важно заложить понятную структуру, но не перегрузить проект инфраструктурой.

## Работы

- Создать корневую структуру проекта.
- Создать отдельный Git-репозиторий `logrus-api`.
- Создать отдельный Git-репозиторий `logrus-bff`.
- Создать директорию Angular frontend внутри `logrus-bff`.
- Выбрать build layout: независимый Gradle Kotlin DSL build в каждом backend-репозитории.
- Настроить единые версии Kotlin, Spring Boot и Java.
- Добавить базовые `.gitignore`, `.editorconfig`, README.
- Добавить профили конфигурации `local`, `test`.
- Зафиксировать convention по package names.
- Зафиксировать правило: общий код между `logrus-api` и `logrus-bff` не выносится в shared module на Phase 1.

## Предлагаемая структура

```text
logrus/
  logrus-api/       # отдельный Git repo
    settings.gradle.kts
    build.gradle.kts
  logrus-bff/       # отдельный Git repo
    settings.gradle.kts
    build.gradle.kts
    logrus-ui/      # Angular frontend
    src/main/resources/static/ # собранная статика Angular для раздачи через BFF
  logrus-infra/     # placement TBD: отдельный repo или локальная infra-директория
  logrus-docs/      # placement TBD: отдельный repo или локальная docs-директория
```

## Зависимости

Нет. Это стартовая задача фазы.

## Результат

- Проект можно открыть в IDE.
- `logrus-api` и `logrus-bff` собираются как независимые Gradle-проекты.
- Frontend-директория `logrus-bff/logrus-ui` готова к инициализации Angular.
- Понятно, где живет инфраструктура и документация.

## Acceptance Criteria

- `./gradlew build` проходит отдельно внутри `logrus-api`.
- `./gradlew build` проходит отдельно внутри `logrus-bff`.
- В `logrus-api` и `logrus-bff` есть минимальные Spring Boot приложения.
- Package naming зафиксирован: `ru.logrus.api` и `ru.logrus.bff`.
- README описывает структуру проекта.
- В `logrus-bff` зафиксировано, что Angular UI живет в `logrus-ui`, а собранная статика раздается самим BFF.

## Не входит

- Реальная бизнес-логика.
- Подключение источников.
- Полная схема БД.
- Production deployment.
