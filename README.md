## API проекта Yatube (v1)
В проекте используются **JWT-токены** для аутентификации. Работа с JWT-токенами организована при помощи библиотеки **Djoser**.
В проекте реализован поиск по подпискам по параметру search с помощью встроенного бэкенда **SearchFilter**, который идёт в составе библиотеки **filters**.

### Как запустить проект:
Клонировать репозиторий и перейти в него в командной строке:
```
git clone https://github.com/miscanth/api_final_yatube.git
```
```
cd api_final_yatube
```
Cоздать и активировать виртуальное окружение:
```
python3 -m venv venv
```
```
source venv/bin/activate
```
Установить зависимости из файла requirements.txt:
```
python3 -m pip install --upgrade pip
```
```
pip install -r requirements.txt
```
Выполнить миграции:
```
python3 manage.py migrate
```
Запустить проект:
```
python3 manage.py runserver
```