# Команды Django

[Источник](https://djbook.ru/rel1.9/ref/django-admin.html#check-appname-appname)

| Серевер                                    |                                                                                                              |
| ------------------------------------------ | ------------------------------------------------------------------------------------------------------------ |
| `python manage.py runserver <HOST>:<PORT>` | Запустить Django сервер. `<HOST>:<PORT>` необязательно                                                       |
| `python manage.py check`                   | Проверить проект на ошибки                                                                                   |
| `python manage.py shell`                   | Запускает интерактивный интерпретатор Python. Можно использовать `-i bpython`, но его нужно установить в ВО. |

| Создание                                    |                                                                                                                                         |
| ------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `python manage.py startapp <ИмяПриложения>` | Создать приложение по шаблону `/venv/lib/python3.9/site-packages/django/conf/app_template`. Не забудьте его добавить в `INSTALLED_APPS` |
| `django-admin startproject <ИмяПриложения>` | Создать проект по шаблону `/venv/lib/python3.9/site-packages/django/conf/project_template`.                                             |
| `python manage.py collectstatic`            | Скопировать все статические файлы из приложений в папку указанную в `proj/settings.py->STATIC_ROOT`                                     |
| `python manage.py createsuperuser`         | Создать админа                                                                                                             |

| БД                                                            |                                                                                        |
| ------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| `python manage.py loaddata ДамФайл.json`                      | Воссоздать БД из `dump`файла                                                           |
| `python manage.py dumpdata Приложение.Модель > ДампФайл.json` | Получить данные для восстановления БД - `dump`                                         |
| `python manage.py flush --database <ИмяБд>`                   | Отчистить данные из указанную БД. `<ИмяБд>` брать из `proj/settings.py->DATABASES{}`   |
| `python manage.py inspectdb`                                  | Получить все модели БД в консоль                                                       |
| `python manage.py makemigrations <app>`                       | Создать у приложения миграцию БД. Без аргументов создает миграции БД у всех приложений |
| `python manage.py migrate <app>`                              | Синхронизирует состояние базы данных, с текущим состоянием миграций.                   |



| Команда                       | Описание |
| ----------------------------- | -------- |
| django-admin compilemessages  |          |
| django-admin createcachetable |          |
| django-admin dbshell          |          |
| django-admin diffsettings     |          |
| makemessages                  |          |
| sendtestemail                 |          |
| showmigrations                |          |
| sqlflush                      |          |
| sqlmigrate                    |          |
| sqlsequencereset              |          |
| squashmigrations              |          |
| test                          |          |
| testserver                    |          |
