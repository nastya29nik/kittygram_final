# Проект Kittygram 🐈

## Описание проекта
**Kittygram** — это социальная сеть для обмена фотографиями любимых питомцев. 
Здесь пользователи могут зарегистрироваться, создать профиль своего котика, добавить его фотографию, указать породу и цвет, а также просматривать публикации других пользователей.

**Основные функции:**
- Регистрация и аутентификация пользователей (Djoser).
- Создание, редактирование и удаление записей о своих питомцах.
- Загрузка фотографий котиков (хранятся в Docker-томе `media`).
- Просмотр ленты с питомцами других пользователей.
- Пагинация списка записей.

## Стек технологий
- **Backend:** Python 3.10, Django 3.2, Django REST Framework, Djoser, Gunicorn
- **Frontend:** React, Node.js
- **База данных:** PostgreSQL 13
- **Веб-сервер:** Nginx
- **Контейнеризация:** Docker, Docker Compose
- **CI/CD:** GitHub Actions (автоматическое тестирование, сборка образов, отправка на Docker Hub и уведомления в Telegram)

---

## Как развернуть проект локально

### 1. Клонирование репозитория
Склонируйте проект на свой компьютер и перейдите в его директорию:
```bash
git clone https://github.com/nastya29nik/kittygram_final.git
cd kittygram_final
```

### 2. Настройка переменных окружения
В корневой директории проекта (рядом с файлом `docker-compose.yml`) создайте файл `.env` и заполните его по следующему шаблону:

```env
# Настройки базы данных PostgreSQL
POSTGRES_USER=kittygram_user
POSTGRES_PASSWORD=kittygram_password
POSTGRES_DB=kittygram
DB_HOST=db
DB_PORT=5432

# Настройки Django
SECRET_KEY=ваш_секретный_ключ_django
ALLOWED_HOSTS=127.0.0.1,localhost,ваш_домен
```

### 3. Запуск Docker-контейнеров
Выполните команду для сборки и запуска контейнеров в фоновом режиме:
```bash
docker compose up -d --build
```

### 4. Применение миграций и сборка статики
После успешного запуска контейнеров, выполните миграции базы данных и соберите статические файлы:
```bash
# Выполняем миграции
docker compose exec backend python manage.py migrate

# Собираем статику бэкенда
docker compose exec backend python manage.py collectstatic --no-input

# Копируем статику в нужную директорию для Nginx
docker compose exec backend cp -r /app/collected_static/. /backend_static/
```

### 5. Создание суперпользователя
Для доступа к админ-панели Django создайте суперпользователя:
```bash
docker compose exec backend python manage.py createsuperuser
```

После выполнения этих шагов проект будет доступен по адресу: http://localhost:9000/.
Админ-панель доступна по адресу: http://localhost:9000/admin/
