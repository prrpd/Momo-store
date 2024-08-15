# Дипломный проект: Магазин пельменей
<img width="900" alt="image" src="https://user-images.githubusercontent.com/9394918/167876466-2c530828-d658-4efe-9064-825626cc6db5.png">

Это дипломный проект по созданию и развертыванию инфраструктуры и приложения для интернет-магазина пельменей.

## Структура проекта

Проект состоит из двух основных репозиториев, каждый из которых хранится в GitLab:

1. **Инфраструктура**: [Репозиторий инфраструктуры](https://gitlab.praktikum-services.ru/std-027-58/infra-momo.git)
2. **Приложение**: [Репозиторий приложения](https://gitlab.praktikum-services.ru/std-027-58/momo-store.git)

### 1. Инфраструктура

В репозитории инфраструктуры с помощью Terraform создаются и настраиваются следующие инфраструктурные объекты:

- **Kubernetes кластер** и рабочие ноды
- **Сеть**: виртуальная сеть, подсети и правила доступа
- **DNS зона**
- **S3 хранилище** для загрузки картинок сайта
- **Сервисные учетные записи**
- **ArgoCD**, **nginx ingress controller**, **prometheus-stack**

#### Использование Helm и ArgoCD

Также в инфраструктурном репозитории содержатся манифесты Kubernetes и созданные на их основе Helm чарты. Эти чарты упаковываются в артефакты с помощью GitLab Runner и отправляются в Nexus для дальнейшего использования в ArgoCD (хотя можно настроить деплоймент без ArgoCD - из gitlab runner с помощью helm upgrade --install).

### 2. Приложение

В репозитории приложения реализован CI/CD процесс, который включает в себя:

- **Анализ кода на уязвимости** с использованием SonarQube и SAST
- **Сборку Docker образов** для бэкэнда и фронтэнда на основе Dockerfile
- **Публикацию контейнеров** в GitLab Container Registry на этапе релиза

## Процесс развертывания

После развертывания инфраструктуры, внешний IP-адрес балансировщика, созданного автоматически при установке nginx ingress controller, указывается в A-записи DNS зоны, соответствующей домену магазина пельменей.

Далее, импортируются сохранённые настройки ArgoCD:

```bash
argocd admin import - < backup.yaml -n argo-cd
```

ArgoCD автоматически загружает последние версии Helm чартов из Nexus и применяет их для развертывания приложения. После этого сайт становится доступным для посещения и оформления заказов.

## Мониторинг и Логирование

Мониторинг и логирование приложения реализованы с помощью **prometheus-stack**. Настроена загрузка метрик с бэкэнда с использованием сущности **ServiceMonitor**, где указаны путь к метрикам и частота опроса.

Данные метрики можно просматривать в **Grafana** через созданные графики. Также доступны графики, отображающие работу всего кластера и контейнеров приложения.

![](https://gitlab.praktikum-services.ru/std-027-58/infra-momo/-/blob/diplom/monitoring/deployed%20app%20metrics.png)



<img width="900" alt="image2" src="https://gitlab.praktikum-services.ru/std-027-58/infra-momo/-/blob/diplom/monitoring/deployed%20app%20metrics.png"/>



<img width="900" alt="image" src="https://gitlab.praktikum-services.ru/std-027-58/infra-momo/-/blob/diplom/monitoring/nginx%20ingress%20controller.jpg">
<img width="900" alt="image" src="https://gitlab.praktikum-services.ru/std-027-58/infra-momo/-/blob/diplom/monitoring/pod.jpg">
![Метрики ingress контроллера](https://gitlab.praktikum-services.ru/std-027-58/infra-momo/-/blob/diplom/monitoring/nginx%20ingress%20controller.jpg)
![Метрики пода](https://gitlab.praktikum-services.ru/std-027-58/infra-momo/-/blob/diplom/monitoring/pod.jpg)

## Заключение

Этот проект представляет собой комплексное решение для автоматизации развертывания и мониторинга веб-приложения на основе Kubernetes, с использованием инструментов CI/CD, инфраструктуры как код (IaC), мониторинга и логирования.