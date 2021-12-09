# Django rest framework

Другие уроки:

- [Плейлист про DRF](https://www.youtube.com/watch?v=iVaqAvlEUbw&list=PLF-NY6ldwAWqSxUpnTBObEP21cFQxNJ7C&index=2)

---

Установка:

```python
pip install djangorestframework pytz
```

---

1. Добавить в `Name_proj->settings.py->INSTALLED_APPS`

    ```python
    INSTALLED_APPS = [
    	...
    	"rest_framework",
    	...
    ]
    ```

1. Добавить в `Name_proj->urls.py->urlpatterns`

    ```python
    from django.conf.urls.static import static
    from django.contrib import admin
    from django.urls import path, include

    from re_view import settings

    urlpatterns = [
    		path('admin/', admin.site.urls),
    		...
    		path("api-auth/",include('rest_framework.urls'))
    		...
    		# Рекомендуемый формат `url` для `Rest` api/v<Версия:int>/<ИмяApi:str>
    		path("api/v1/ЛюбоеИмя/",include('rest_framework.urls'))
    ]
    ```

1. Создаем файл `serializers.py` (!!!)

    ```python
    from rest_framework import serializers
    from .models import *


    class <ИмяСериализованнйМодели>(serializers.ModelSerializer):

    	# Можно получать связанные данные столбцов из главной таблице. `read_only=True` указывает что данные  только для чтения.
    	# Не забудте добаить этот атриубт в `fields`
    	# Если вы используете данные из разных таблиц, то объединяйте запрос, дабы не делать лишних запросов к БД
    	# <ИмяАтрибутаВ_Модели> = serializers.SlugRelatedField(slug_field="<ИмяСтольцаВ_ГлавнойТаблице>", read_only=True)

    	class Meta:
    		# Имя модели
    		model = <ИмяМоделиБд>
    		# Столбцы которые мы хотим сериализовать и показывать
    		fields = ('',)

    ```

---

## Views

```python
from rest_framework.response import Response
from rest_framework.request import Request
from rest_framework.views import APIView
from .models import *
from .serializers import *


class <ЛюбоеИмяКласса>(APIView):
	# Текст указанный в первом многострочном кометарри отображается в `REST`
	"""Любой текст 111"""

	http_method_names = ["get", "post", ]  # Список методов HTTP, которые обрабатывает класс.
	model = Person # Какую модель используем

	def get(self, request: Request):
		"""
		В методе обрабатывается GET запрос
# 	request.method == "GET"
		"""
		# Получаем данные из БД
		<QurySet/RawSet> = self.model.objects.<ЗапросВ_БД>
		# Сериализуем данные
		serializers = <ИмяСериализованнйМодели>(<QurySet/RawSet>, many=True) # Если у вас больше одного объекта то ` many=True`
		# Возвращаем страницу `djangorestframework`
		return Response(serializers.data)
```

![Вывод `rest_framework.response`](_attachments/2aedba94edf028e5822c28b0b56237b7.png)

## CRUD

### Create

#### Вариант на `ORM`:

Используя класс `CreateAPIView` у нас появляется возможность заполнять данные ерез удобные формы

`serializers.py`

```python
class <ИмяСериализованнйМодели>(serializers.ModelSerializer):
	class Meta:
		# Имя модели
		model = Person
		# Столбцы которые мы хотим сериализовать и показывать
		fields = "__all__"
```

`views.py`

```python
from rest_framework.generics import CreateAPIView

class <ЛюбоеИмяКласса>(CreateAPIView):

	http_method_names = ["post", ]  # Список методов HTTP, которые обрабатывает класс.

	model = Person  # Какую модель используем
	serializer_class = <ИмяСериализованнйМодели> # Класс из `serializers.py`

	# `post` запрос уже обрбатывается
	# def post(self, request: Request, *args, **kwargs):
	# 	review = Set_Serel(data=request.data)
	# 	if review.is_valid():
	# 		review.save()
	# 		return Response(status=200)
	# 	return Response(status=403)
```

### Read

[Если вы используете данные из разных таблиц, то объединяйте запрос, дабы не делать лишних запросов к БД](https://www.django-rest-framework.org/api-guide/relations/)

#### Вариант на `ORM`:

`serializers.py`

```python
	from rest_framework import serializers
	from .models import *


	class <ИмяСериализованнйМодели>(serializers.ModelSerializer):

		# Можно получать связанные данные столбцов из главной таблице. `read_only=True` указывает что данные  только для чтения.
		# Не забудте добаить этот атриубт в `fields`
		# Если вы используете данные из разных таблиц, то объединяйте запрос, дабы не делать лишних запросов к БД
		<ИмяАтрибутаВ_Модели> = serializers.SlugRelatedField(slug_field="<ИмяСтольцаВ_ГлавнойТаблице>", read_only=True)

		class Meta:
			# Имя модели
			model = <ИмяМоделиБд>
			# Столбцы которые мы хотим сериализовать и показывать
			fields = ('<ИмяАтрибутаВ_Модели>',)
```

`views.py`

```python
from rest_framework.response import Response
from rest_framework.request import Request
from rest_framework.views import APIView
from .models import *
from .serializers import *


class <ЛюбоеИмяКласса>(APIView):
	# Текст указанный в первом многострочном кометарри отображается в `REST`
	"""Любой текст 111"""

	http_method_names = ["get", "post", ]  # Список методов HTTP, которые обрабатывает класс.
	model = Person # Какую модель используем

	def get(self, request: Request):
		"""
		В методе обрабатывается GET запрос
		request.method == "GET"
		"""
		# Получаем данные из БД
		# `select_related('<ИмяАтрибутаВ_Модели>')` выполняет `JOIN`
		<QurySet/RawSet> = self.model.objects.select_related('<ИмяАтрибутаВ_Модели>').<ЗапросВ_БД> # !!!
		# Сериализуем данные
		serializers = <ИмяСериализованнйМодели>(<QurySet/RawSet>, many=True) # Если у вас больше одного объекта то ` many=True`
		# Возвращаем страницу `djangorestframework`
		return Response(serializers.data)
```

#### Вариант на Чистом `SQL`:

`serializers.py`

```python
	from rest_framework import serializers
	from .models import *


	class <ИмяСериализованнйМодели>(serializers.ModelSerializer):

		class Meta:
			# Имя модели
			model = <ИмяМоделиБд>
			# Столбцы которые мы хотим сериализовать и показывать
			# Если мы хотим отображать поля из других таблиц то их нужно явно указать в `fields`
			# !!! Создайте атрибут в модели с таким же именем как и в столбце из другой таблице
			fields = ('<ИмяАтрибутаВ_Модели>','<username>')
```

`views.py`

```python
from rest_framework.response import Response
from rest_framework.request import Request
from rest_framework.views import APIView
from .models import *
from .serializers import *


class <ЛюбоеИмяКласса>(APIView):
	# Текст указанный в первом многострочном кометарри отображается в `REST`
	"""Любой текст 111"""

	http_method_names = ["get", "post", ]  # Список методов HTTP, которые обрабатывает класс.
	model = Person # Какую модель используем

	def get(self, request: Request):
		"""
		В методе обрабатывается GET запрос
		request.method == "GET"
		"""
		# Получаем данные из БД
		# `select_related('<ИмяАтрибутаВ_Модели>')` выполняет `JOIN`
		<QurySet/RawSet> = self.model.objects.raw("""SELECT ... <username> ... FROM ... JOIN ... ON ... """)
		# Сериализуем данные
		serializers = <ИмяСериализованнйМодели>(<QurySet/RawSet>, many=True) # Если у вас больше одного объекта то ` many=True`
		# Возвращаем страницу `djangorestframework`
		return Response(serializers.data)
```

`models.py`

```python
class <ИмяМодели>(models.Model):
	objects = None

	...
	# Люые реальные столбцы
	name = models.<CharField(max_length=255, verbose_name="Имя Пользователя")>
	...

	# Пустяа перменная специально для `serializers.py` чтобы он учитывал такой атрибут
	# А после, используя чистые SQL запросы, мы можем поместить любые значения
	<username> = None # !!!
```

# `validators` Валидация данных

Установка:

```bash
pip install validators
```

| Функция                                                                | Описание                                                                       |
| ---------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| validators.email(`'<Текст>'`)                                          | Это почта                                                                      |
| validators.between(`<ПроверитьЧисло>,min=<МинЧисло>,max=<МаксЧисло>)`) | Число находится между границ                                                   |
| validators.length(`<Строка>,min=<МинСимволов>,max=<МксСимволов>`)      | Строка находится мужду границ                                                  |
| ---                                                                    | ---                                                                            |
| validators.domain(`<Domen>`)                                           | Это домен                                                                      |
| validators.ipv4()                                                      |                                                                                |
| validators.ipv6()                                                      |                                                                                |
| validators.mac_address()                                               |                                                                                |
| validators.slug()                                                      |                                                                                |
| validators.url(`<URL>,public=False`)                                   | Это `URL`. Если `public=True` то проверит что `IP` не из списка внетреней сети |
| ---                                                                    |                                                                                |
| validators.truthy(`<Any>`)                                             | Проверить что значение относиться к `True`                                     |

Создать свой валидатор. Если функция вернет `False` это вызавет `ValidationFailure`

```python
@validators.validator
def <ИмяВалидатора>(val):
	return val > 0

if __name__ == '__main__':
	tmp = Zero(-1)
	print(tmp)
```

# `faker` Генератор случайных данных

Установка:

```bash
pip install Faker
```

```python
from faker import Faker
# Создать объект, указать язык вывода
fake = Faker(locale= "ru_RU")
# Можно задать Сид
Faker.seed(<Число>)



tmp = fake.name()
```

| Метод                                                       | Описание                                                                                  |
| ----------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| name()                                                      | Имя                                                                                       |
| text(`<СколькоСимволов>,<[СпискоДоступныхСлов,]>`)          | Текст                                                                                     |
| paragraph(`<СколькоПараграфиов>, <[СпискоДоступныхСлов,]>`) | Случайны текст с указанным количеством параграфов                                         |
| word(`<[СпискоДоступныхСлов,]>`)                            | Слово                                                                                     |
| words(`<СколькоСлов>,<[СпискоДоступныхСлов,]>`)             | Несколько слов                                                                            |
| ---                                                         | ---                                                                                       |
| company()                                                   | Компания                                                                                  |
| address()                                                   | Адрес проживания                                                                          |
| job()                                                       | Работа                                                                                    |
| date()                                                      | Дата                                                                                      |
| phone_number()                                              | Номер телефона                                                                            |
| user_name()                                                 | Ник                                                                                       |
| password()                                                  | Пароль                                                                                    |
| ---                                                         | ---                                                                                       |
| color()                                                     | Цвет                                                                                      |
| file_name()                                                 | Имя файла                                                                                 |
| file_path()                                                 | Путь к файлу                                                                              |
| image_url()                                                 | `URL` фото                                                                                |
| slug()                                                      | Слаг                                                                                      |
| uri()                                                       | `URL`                                                                                     |
| uri_page()                                                  | Имя страницы для `URL`                                                                    |
| uri_path()                                                  | Путь к страницу для `URL`                                                                 |
| url(`СписокURl`)                                            | Выбрать случайный `URL` из списка                                                         |
| image()                                                     | Бинарное изображение                                                                      |
| json()                                                      | `JSON`                                                                                    |
| ---                                                         | ---                                                                                       |
| pydict()                                                    | Словарь [+](https://faker.readthedocs.io/en/master/providers/faker.providers.python.html) |
| pyiterable()                                                | Случайная структура данных                                                                |
| pylist()                                                    | Список                                                                                    |
| pyset()                                                     | Множество                                                                                 |
| pystruct()                                                  | Объект                                                                                    |
| pytuple()                                                   | Картеж                                                                                    |
