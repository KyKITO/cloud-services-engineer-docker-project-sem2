# Проектная работа дисциплины "Docker-контейнеризация и хранение данных"

## Обоснование сборки образов
В данном проекте реализованы оптимизированные multi-stage builds для обеих компонентов с использованием легковесных базовых образов. Такой подход обеспечивает:
* Минимальный размер финальных образов
* Улучшенную безопасность за счет отсутствия инструментов разработки
* Оптимизированное кэширование Docker layers
* Production-ready контейнеры без лишних зависимостей

### Таблица с размерами образов

| Компонент | Название образа                        |Размер  |Оптимизация                          |
|-----------|----------------------------------------|--------|-------------------------------------|
| Backend   | `kykito/docker-project-backend:v1.0.0` |6,85 Mb |Multi-stage + Alpine + Static binary |
| Frontend  | `kykito/docker-project-frontend:v1.0.0`|19,97 Mb|Multi-stage + Nginx Alpine + Gzip    |

## Локальный запуск

### Установите Docker:
```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

### Запуск docker compose
```bash
docker compose --profile production build
docker compose --profile production up -d
```