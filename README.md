# MyApp Helm Chart

Универсальный типовой Helm-чарт для деплоя приложений в Kubernetes.

## Использование

### Установка
```bash
helm install my-release ./myapp -n default
```

### Рендеринг манифестов без установки
```bash
helm template my-release ./myapp
```

### Параметры
Все параметры задаются в файле `values.yaml`.  
Основные категории:
- `image.*` — контейнерный образ и теги
- `service.*` — сервис и порты
- `ingress.*` — правила доступа и TLS
- `autoscaling.*` — настройки HPA
- `persistence.*` — хранилище (PVC)
- `job.*` и `cronjob.*` — для фоновых задач
- `serviceMonitor.*` — интеграция с Prometheus Operator

## Лицензия

[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](./LICENSE)

This Helm Chart is licensed under the [Apache License 2.0](./LICENSE).  
Original source: [github.com/SterhLight/your-repo](https://github.com/SterhLight/your-repo)

(c) 2025 SterhLight Inc. av.popytaev (krasava)
