# App Helm Chart

Универсальный **типовой Helm-чарт** для деплоя приложений в Kubernetes **без внешних зависимостей**.
Поддерживает: Deployment, Service, Ingress, HPA, Job, CronJob, PVC, PDB, NetworkPolicy, ServiceMonitor.

---

## Быстрый старт

Установка в namespace default
```bash
helm install my-release ./myapp -n default
```

Рендеринг манифестов без установки
```bash
helm template my-release ./myapp
```

---

## Конфигурация (values.yaml)

### Глобальные дефолты образа
```yaml
image:
  repository: nginx
  tag: "1.27"
  pullPolicy: IfNotPresent
  pullSecrets: []
```

### Переопределение образов по ресурсам

Для **Deployment / Job / CronJob** можно указать свой `image.*`. Если параметр не задан — берётся из `image.*` (глобально).

Прочие важные параметры:
- `service.*` — тип сервиса, порт/targetPort, аннотации;
- `ingress.*` — правила, класс, TLS;
- `autoscaling.*` — HPA v2 (CPU/Memory);
- `persistence.*` — PVC или existingClaim;
- `serviceMonitor.*` — интеграция с Prometheus Operator;
- `enablePDB` и `pdb.*`, `networkPolicy.*` — отказоустойчивость и сеть.

Полный список см. в `values.yaml`.

---

## Примеры

### Разные образы для всех сущностей
```yaml
image:
  repository: myorg/base
  tag: "1.0.0"

deployment:
  image:
    repository: myorg/api
    tag: "2.1.0"

job:
  enabled: true
  name: "db-migrate"
  image:
    repository: myorg/migrate
    tag: "0.3.0"
  command: ["/bin/sh","-c"]
  args: ["./migrate up"]

cronjob:
  enabled: true
  name: "cleanup"
  schedule: "0 3 * * *"
  image:
    repository: myorg/cleanup
    tag: "1.2.0"
  jobTemplate:
    args: ["--days=14"]
```

## Проверка

```bash
helm lint ./myapp
helm template my-release ./myapp | kubectl apply --dry-run=client -f -
```

---

## Структура
```
myapp/
  Chart.yaml
  values.yaml
  templates/
    _helpers.tpl
    deployment.yaml
    service.yaml
    ingress.yaml
    hpa.yaml
    serviceaccount.yaml
    configmap.yaml
    secret.yaml
    pvc.yaml
    pdb.yaml
    networkpolicy.yaml
    servicemonitor.yaml
    job.yaml
    cronjob.yaml
    NOTES.txt
```

---

## Лицензия и атрибуция

[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](./LICENSE)

Этот чарт распространяется по лицензии **Apache License 2.0**. См. файл [`LICENSE`](./LICENSE).  
Оригинальный репозиторий: **https://github.com/SterhLight/<your-repo>** _(замени на фактический URL)._

© 2025 SterhLight Inc. av.popytaev (krasava)

---

## Maintainers

- **SterhLight Inc. av.popytaev (krasava)** — основной мейнтейнер
  - GitHub: https://github.com/SterhLight

