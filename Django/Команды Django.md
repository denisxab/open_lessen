# Команды Django

[Источник](https://djbook.ru/rel1.9/ref/django-admin.html#check-appname-appname)

| Команда                                     | Описание                                                                                                                                |
| ------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `python manage.py check`                    | Проверить проект на ошибки                                                                                                              |
| django-admin compilemessages                |                                                                                                                                         |
| django-admin createcachetable               |                                                                                                                                         |
| django-admin dbshell                        |                                                                                                                                         |
| django-admin diffsettings                   |                                                                                                                                         |
| dumpdata                                    |                                                                                                                                         |
| `python manage.py flush --database <ИмяБд>` | Отчистить данные из указанную БД. `<ИмяБд>` брать из `proj/settings.py->DATABASES{}`                                                    |
| `python manage.py inspectdb`                | Получить все модели БД в консоль                                                                                                        |
| loaddata                                    |                                                                                                                                         |
| makemessages                                |                                                                                                                                         |
| `python manage.py makemigrations <app>`     | Создать у приложения миграцию БД. Без аргументов создает миграции БД у всех приложений                                                  |
| `python manage.py migrate <app>`            | Синхронизирует состояние базы данных, с текущим состоянием миграций.                                                                    |
| `ptrhon manage.py runserver <HOST>:<PORT>`  | Запустить Django сервер. `<HOST>:<PORT>` необязательно                                                                                  |
| sendtestemail                               |                                                                                                                                         |
| `python manage.py shell`                    | Запускает интерактивный интерпретатор Python. Можно использовать `-i bpython`, но его нужно установить в ВО.                            |
| showmigrations                              |                                                                                                                                         |
| sqlflush                                    |                                                                                                                                         |
| sqlmigrate                                  |                                                                                                                                         |
| sqlsequencereset                            |                                                                                                                                         |
| squashmigrations                            |                                                                                                                                         |
| `python manage.py startapp <ИмяПриложения>` | Создать приложение по шаблону `/venv/lib/python3.9/site-packages/django/conf/app_template`. Не забудьте его добавить в `INSTALLED_APPS` |
| `django-admin startproject <ИмяПриложения>` | Создать проект по шаблону `/venv/lib/python3.9/site-packages/django/conf/project_template`.                                             |
| test                                        |                                                                                                                                         |
| testserver                                  |                                                                                                                                         |
| `python manage.py collectstatic`            | Скопировать все статические файлы из приложений в папку указанную в `proj/settings.py->STATIC_ROOT`                                     |
