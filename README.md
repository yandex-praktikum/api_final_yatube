### Описание
API-сервис приложения социальной сети **Yatube**.
Данный сервис позволяет через стандартные HTTP запросы выполнять следующие команды:

#### ![Изображение незарегистрированного пользователя](https://cdn2.scratch.mit.edu/get_image/user/60928233_60x60.png) Для незарегистрированных пользователей:
  - Посты:
    - Просмотр списка постов: **GET**-запрос на адрес `http://127.0.0.1:8000/api/v1/posts/`
    - Просмотр конкретного поста: **GET**-запрос на адрес `http://127.0.0.1:8000/api/v1/posts/{номер_публикации}/`
  - Комментарии к постам:
    - Просмотр списка комментариев к посту: **GET**-запрос на адрес `http://127.0.0.1:8000/api/v1/posts/{номер_публикации}/comments/`
    - Просмотр комментария к посту: **GET**-запрос на адрес `http://127.0.0.1:8000/api/v1/posts/{номер_поста}/comments/{номер_комментария}/`
  - Группы:
    - Просмотр списка сообществ: **GET**-запрос на адрес `http://127.0.0.1:8000/api/v1/groups/`
    - Просмотр конкретного сообщества: **GET**-запрос на адрес `http://127.0.0.1:8000/api/v1/groups/{номер_сообщества}/`
    
#### ![Изображение зарегистрированного пользователя](https://yumtrade.ru/35929-33226-medium/modul-verifikacija-lic.jpg) Для зарегистрированных пользователей:
  - Токен:
    - Получение токена: **POST**-запрос на адрес `http://127.0.0.1:8000/api/v1/jwt/create/` с передачей `username`(строка) и `password`(строка) зарегистрированного пользователя; ответ будет содержать ***access-токен*** (используется для выполнения запросов, требующих авторизации [помечены далее звзедочкой]) и ***refresh-токен*** (используется для обновления access-токена).
    - Обновление токена: **POST**-запрос на адрес `http://127.0.0.1:8000/api/v1/jwt/refresh/` с передачей в теле запроса ***refresh-токена*** в формате строки по ключу `"refresh"`
    - Проверка токена[^1]: **POST**-запрос на адрес `http://127.0.0.1:8000/api/v1/jwt/verify/` с передачей в теле запроса проверяемого ***access-*** либо ***refresh-токена*** в формате строки по ключу `"token"`
  - Посты:
    - Просмотр списка постов: **GET**-запрос на адрес `http://127.0.0.1:8000/api/v1/posts/`
    - Просмотр конкретного поста: **GET**-запрос на адрес `http://127.0.0.1:8000/api/v1/posts/{номер_публикации}/`
    - Добавление поста[^1] **POST**-запрос на адрес `http://127.0.0.1:8000/api/v1/posts/` с передачей в теле запроса ключей со значениями:
      - `"text"` - строка символов;
      - `"image"` - строка символов или null(ничего) - необязательное поле;
      - `"group"` - идентификационный (id) номер сообщества - необязательное поле.
    - Изменение своей публикации[^1]: **PATCH**- (для частичого изменения) или **PUT**-запрос (для полного изменения) на адрес `http://127.0.0.1:8000/api/v1/posts/{номер_публикации}/` с передачей в теле запроса ключей со значениями изменяемых полей:
      - `"text"` - строка символов;
      - `"image"` - строка символов или null(ничего);
      - `"group"` - идентификационный (id) номер сообщества.     
    - Удаление своей публикации[^1]: **DELETE**-запрос на адрес `http://127.0.0.1:8000/api/v1/posts/{номер_публикации}/`
  - Комментарии к постам:
    - Просмотр списка комментариев к публикации: **GET**-запрос на адрес `http://127.0.0.1:8000/api/v1/posts/{номер_публикации}/comments/`
    - Просмотр конкретного комментария к публикации: **GET**-запрос на адрес `http://127.0.0.1:8000/api/v1/posts/{номер_публикации}/comments/{номер_комментария}/`
    - Написание комментария к публикации[^1]: **POST**-запрос на адрес `http://127.0.0.1:8000/api/v1/posts/{номер_публикации}/comments/` с передачей в теле запроса текста по ключу `"text"`
    - Изменение своего комментария к публикации[^1]: **PATCH**- (для частичого изменения) или **PUT**-запрос (для полного изменения) на адрес `http://127.0.0.1:8000/api/v1/posts/{номер_публикации}/comments/{номер_комментария}/` с передачей в теле запроса текста по ключу `"text"`
    - Удаление своего комментария к публикации[^1]: **DELETE**-запрос на адрес `http://127.0.0.1:8000/api/v1/posts/{номер_публикации}/comments/{номер_комментария}/`
  - Группы:
    - Просмотр списка сообществ: **GET**-запрос на адрес `http://127.0.0.1:8000/api/v1/groups/`
    - Просмотр конкретного сообщества: **GET**-запрос на адрес `http://127.0.0.1:8000/api/v1/groups/{номер_сообщества}/`

### Установка
Как запустить проект:
Клонировать репозиторий и запустить его в командной строке:
```
git clone https://github.com/orel333?tab=repositories
```

Зайти в папку с проектом - корневой каталог, находясь в нём, создать и активировать виртуальное окружение:
```
python -m venv venv
source venv/Scripts/activate
```

Установить зависимости из файла *requirements.txt*:
```
pip install -r requirements.txt
```

Зайти в папку проекта (которая содержит файл *manage.py*)
Выполнить миграции:
```
python manage.py migrate
```

Запустить проект:
```
python manage.py runserver
```

### Примеры:
Получаем токен:
```
POST http://127.0.0.1:8000/api/v1/jwt/create/
Content-Type: application/json

{
    "username": "Hunk",
    "password": "some0990"
}
```
Ответ:
```
200 OK
{
  "refresh": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoicmVmcmVzaCIsImV4cCI6MTY0Mzc5NzM0OSwianRpIjoiZWNiY2RlYmIwMTNjNDA5NWFhNDY2ZjY3YzczN2MwZDMiLCJ1c2VyX2lkIjo1fQ.szixudq4h0xe0hbQDrr-sRfWNxs6qP8_aMlegrqyXK4",
  "access": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjQzNzExMjQ5LCJqdGkiOiI2MDU2NjQ4ODM5YTY0NTI4YTBjMDk0OGE0NDk5ODc0MSIsInVzZXJfaWQiOjV9.H5t6o3FevmOkpzpzrrKwfkuq_LdI-k4_soVDB6W1G0g"
}
```

Создаём пост:
```
POST http://127.0.0.1:8000/api/v1/posts/
Content-Type: application/json
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl9k94BlIjoiYWNjZXNzIiwiZXhwIjoxNjQzNzQwODY0LCJqdGkik94iZGE1OTIwZjM5NDI0NzcwOWQxODYzMzExNTU0NTlkZiIsInVzZXJfaWQiOjV9.AxKHmWFC-_V8dGbkKzsnyb28zaA5bizjfNBLPvSI6TE

{
    "text": "This new day and a new post",
    "image": null,
    "group": 2
}
```
Ответ:
```
200 OK
{
  "id": 28,
  "author": "Hunk",
  "text": "This new day and a new post",
  "pub_date": "2022-02-01T18:36:43.952786Z",
  "image": null,
  "group": 2
}
```
Создаём комментарий к публикации:
```
POST http://127.0.0.1:8000/api/v1/posts/28/comments/
Content-Type: application/json
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBgh8oiYWNlo4NzIiwiZXhwIjoxNjQzNzQxMTM2LCJqdGkiOiI4NTNmNzlkZTlkMzQ0NTZhYTg2YWQ1YmU0OTQ2ZmI4NyIsInVzZXJfaWQiOjR9.MP3DxwhV6-KiJCABLDLRC2VcVOsnC3QWy4_yU70wOO8

{
    "text": "I'm your fan!"
}
```
Ответ:
```
200 OK
{
  "id": 4,
  "author": "Alien",
  "text": "I'm your fan!",
  "created": "2022-02-01T18:40:51.635407Z",
  "post": 28
}
```
Корректируем комментарий к посту (доступно только автору комментария):
```
PATCH http://127.0.0.1:8000/api/v1/posts/28/comments/4/
Content-Type: application/json
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBgh8oiYWNlo4NzIiwiZXhwIjoxNjQzNzQxMTM2LCJqdGkiOiI4NTNmNzlkZTlkMzQ0NTZhYTg2YWQ1YmU0OTQ2ZmI4NyIsInVzZXJfaWQiOjR9.MP3DxwhV6-KiJCABLDLRC2VcVOsnC3QWy4_yU70wOO8

{
    "text": "Bo-oring!"
}
```
Ответ:
```
200 OK
{
  "id": 4,
  "author": "Alien",
  "text": "Bo-oring!",
  "created": "2022-02-01T18:40:51.635407Z",
  "post": 28
}
```
Удаляем комментарий к посту (доступно только автору комментария):
```
DELETE http://127.0.0.1:8000/api/v1/posts/28/comments/4/
Content-Type: application/json
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBgh8oiYWNlo4NzIiwiZXhwIjoxNjQzNzQxMTM2LCJqdGkiOiI4NTNmNzlkZTlkMzQ0NTZhYTg2YWQ1YmU0OTQ2ZmI4NyIsInVzZXJfaWQiOjR9.MP3DxwhV6-KiJCABLDLRC2VcVOsnC3QWy4_yU70wOO8
```
Ответ (*No content* - подтверждение удаления):
```
204 No Content
```


[^1]: Этот и другие запросы, отмеченные сноской с цифрой 1, должны передаваться со словом Bearer и access-токеном в заголовке запроса после ключа Authorization: (ключ и Bearer с токеном - без двойных кавычек)
