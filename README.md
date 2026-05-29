# Kittygram

Kittygram — учебный сервис для публикации карточек котиков. Пользователи могут регистрироваться, авторизоваться, добавлять котиков, прикреплять изображения и достижения.

Проект подготовлен для запуска в Docker-контейнерах и автоматической сборки образов через GitHub Actions.

## Технологии

- Python 3.9
- Django 3.2
- Django REST Framework
- Djoser
- PostgreSQL 13
- React
- Nginx
- Docker, Docker Compose
- GitHub Actions

## Локальный запуск в Docker

Создайте файл `.env` на основе `.env.example`:

```bash
cp .env.example .env
```

Запустите контейнеры:

```bash
docker compose up --build
```

После запуска проект будет доступен по адресу:

```text
http://localhost:9000/
```

API:

```text
http://localhost:9000/api/
```

Админка:

```text
http://localhost:9000/admin/
```

## Основные переменные окружения

```env
POSTGRES_DB=kittygram
POSTGRES_USER=kittygram_user
POSTGRES_PASSWORD=kittygram_password
DB_HOST=db
DB_PORT=5432
SECRET_KEY=change_me
DEBUG=False
ALLOWED_HOSTS=localhost,127.0.0.1,your_domain
CSRF_TRUSTED_ORIGINS=https://your_domain
STATIC_ROOT=/backend_static/static
MEDIA_ROOT=/media
DOCKERHUB_USERNAME=username
```

## CI/CD

Workflow находится в файлах:

- `.github/workflows/main.yml`
- `kittygram_workflow.yml`

При push в ветку `main` workflow:

1. проверяет backend через `ruff`;
2. запускает backend-тесты;
3. запускает frontend-тесты;
4. собирает Docker-образы;
5. публикует образы на Docker Hub;
6. отправляет уведомление в Telegram.

Для работы workflow нужно добавить secrets в GitHub:

- `DOCKERHUB_USERNAME`
- `DOCKERHUB_TOKEN`
- `TELEGRAM_TOKEN`
- `TELEGRAM_TO`

## Docker Hub images

В workflow собираются и публикуются образы:

- `username/kittygram_backend`
- `username/kittygram_frontend`
- `username/kittygram_gateway`

Замените `username` на свой логин Docker Hub через secret `DOCKERHUB_USERNAME`.

## Production compose

Для запуска на сервере используйте файл:

```bash
docker compose -f docker-compose.production.yml up -d
```

Перед запуском заполните `.env` реальными значениями домена, секретного ключа и логина Docker Hub.
