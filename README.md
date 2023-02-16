# yamdb_final
![yamdb workflow](https://github.com/vmikail/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

# Проект YaMDb
## Описание проекта
Проект YaMDb собирает отзывы пользователей на произведения. Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.
В каждой категории есть произведения: книги, фильмы или музыка. Например, в категории «Книги» могут быть произведения «Винни Пух и все-все-все» и «Марсианские хроники», а в категории «Музыка» — песня «Давеча» группы «Насекомые» и вторая сюита Баха. Произведению может быть присвоен жанр из списка предустановленных (например, «Сказка», «Рок» или «Артхаус»). Новые жанры может создавать только администратор.

Благодарные или возмущённые читатели оставляют к произведениям текстовые отзывы (Review) и выставляют произведению рейтинг (оценку в диапазоне от одного до десяти). Из множества оценок автоматически высчитывается средняя оценка произведения.

Полная документация к API находится по эндпоинту /redoc

Стек технологий использованный в проекте:
- Python 3.7
- Django 2.2.28
- DRF
- JWT

## Запуск проекта в dev-режиме
- Клонировать репозиторий и перейти в него в командной строке.
- Установите виртуальное окружение c учетом версии Python 3.7     (выбираем         python не ниже 3.7):
    python -m venv venv
- Активируйте виртуальное окружение
    source venv/Scripts/activate
- Затем нужно установить все зависимости из файла requirements.txt
    python -m pip install --upgrade pip
    pip install -r requirements.txt
- Выполняем миграции:
    python manage.py migrate

## Запуск проекта
    python manage.py runserver

## Эндпоинты
    Подробная документация доступна по эндпоинту /redoc/

    Неавторизованные пользователи могут только читать информацию.

    Права доступа: Доступно без токена.
    GET /api/v1/categories/ - Получение списка всех категорий
    GET /api/v1/genres/ - Получение списка всех жанров
    GET /api/v1/titles/ - Получение списка всех произведений
    GET /api/v1/titles/{title_id}/reviews/ - Получение списка всех отзывов
    GET /api/v1/titles/{title_id}/reviews/{review_id}/comments/ - Получение списка всех комментариев к отзыву
    Права доступа: Администратор
    GET /api/v1/users/ - Получение списка всех пользователей

## Пользовательские роли
  
    - Аноним — может просматривать описания произведений, читать отзывы    и    комментарии.
    - Аутентифицированный пользователь (user) — может, как и Аноним, читать всё, дополнительно он может публиковать отзывы и ставить оценку произведениям (фильмам/книгам/песенкам), может комментировать чужие отзывы; может редактировать и удалять свои отзывы и комментарии. Эта роль присваивается по умолчанию каждому новому пользователю.
    - Модератор (moderator) — те же права, что и у Аутентифицированного пользователя плюс право удалять любые отзывы и комментарии.
    - Администратор (admin) — полные права на управление всем контентом проекта. - Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.
    - Суперюзер Django — обладает правами администратора (admin)

## Примеры работы с API для авторизованных пользователей

### Добавление категории:

    Права доступа: Администратор.
    POST /api/v1/categories/
    {
    "name": "string",
    "slug": "string"
    }
### Удаление категории:

    Права доступа: Администратор.
    DELETE /api/v1/categories/{slug}/
### Добавление жанра:

    Права доступа: Администратор.
    POST /api/v1/genres/
    {
    "name": "string",
    "slug": "string"
    }
### Удаление жанра:

    Права доступа: Администратор.
    DELETE /api/v1/genres/{slug}/
### Обновление публикации:

    PUT /api/v1/posts/{id}/
    {
    "text": "string",
    "image": "string",
    "group": 0
    }
### Добавление произведения:

    Права доступа: Администратор. 
    Нельзя добавлять произведения, которые еще не вышли (год выпуска не может быть больше текущего).

    POST /api/v1/titles/
    {
    "name": "string",
    "year": 0,
    "description": "string",
    "genre": [
        "string"
    ],
    "category": "string"
    }
### Добавление произведения:

    Права доступа: Доступно без токена
    GET /api/v1/titles/{titles_id}/
    {
    "id": 0,
    "name": "string",
    "year": 0,
    "rating": 0,
    "description": "string",
    "genre": [
        {
        "name": "string",
        "slug": "string"
        }
    ],
    "category": {
        "name": "string",
        "slug": "string"
    }
    }
### Частичное обновление информации о произведении:

    Права доступа: Администратор
    PATCH /api/v1/titles/{titles_id}/
    {
    "name": "string",
    "year": 0,
    "description": "string",
    "genre": [
        "string"
    ],
    "category": "string"
    }
### Частичное обновление информации о произведении:

    Права доступа: Администратор
    DEL /api/v1/titles/{titles_id}/

По TITLES, REVIEWS и COMMENTS аналогично, более подробно по эндпоинту /redoc/

### Работа с пользователями:
    Для работы с пользователя есть некоторые ограничения для работы с ними. Получение списка всех пользователей.

    Права доступа: Администратор
    GET /api/v1/users/ - Получение списка всех пользователей
### Добавление пользователя:

    Права доступа: Администратор
    Поля email и username должны быть уникальными.
    POST /api/v1/users/ - Добавление пользователя
    {
    "username": "string",
    "email": "user@example.com",
    "first_name": "string",
    "last_name": "string",
    "bio": "string",
    "role": "user"
    }
### Получение пользователя по username:

    Права доступа: Администратор
    GET /api/v1/users/{username}/ - Получение пользователя по username
### Изменение данных пользователя по username:

    Права доступа: Администратор
    PATCH /api/v1/users/{username}/ - Изменение данных пользователя по username
    {
    "username": "string",
    "email": "user@example.com",
    "first_name": "string",
    "last_name": "string",
    "bio": "string",
    "role": "user"
    }
### Удаление пользователя по username:

    Права доступа: Администратор
    DELETE /api/v1/users/{username}/ - Удаление пользователя по username
### Получение данных своей учетной записи:

    Права доступа: Любой авторизованный пользователь
    GET /api/v1/users/me/ - Получение данных своей учетной записи
### Изменение данных своей учетной записи:

    Права доступа: Любой авторизованный пользователь
    PATCH /api/v1/users/me/ # Изменение данных своей учетной записи

