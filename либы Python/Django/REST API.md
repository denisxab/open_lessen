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

# Django Ninja

## Введение

`Django Ninja` основан на `Pydantic` [Pydantic](../../Уроки/Блок6%20-%20Сети.md#Pydantic).

---

Установка: [+](https://github.com/vitalik/django-ninja) [Видео от разработчика](https://www.youtube.com/watch?v=GzBdW6jVwV4)

```bash
pip install django-ninja orjson
```

`orjson` самый быстрый парсер для `JSON` в `Python` [+](https://github.com/ijl/orjson#dataclass)

---

1. Создать файл `Nmae_proj->api.py`
   `api.py`
   Стандартный заголовок файла

    ```python
    from ninja import NinjaAPI

    # Использовать быстрый рендер для `json` = `orjson`
    class ORJSONRenderer(BaseRenderer):
    	media_type = "application/json"

    	def render(self, request, data, *, response_status):
    		if isinstance(data, BaseModel):
    			# Для схем и `Pydantic`
    			return orjson.dumps(data.__dict__)
    		# Для всех остальных
    		return orjson.dumps(data)

    # Использовать быстрый парсер для `json` = `orjson`
    class ORJSONParser(Parser):
    	def parse_body(self, request):
    		return orjson.loads(request.body)


    api = NinjaAPI(version='<НомерВерсии>', csrf=True,  parser=ORJSONParser(), renderer=ORJSONRenderer())
    ```

    Функции. (Этижи функции мы можем вызвать в самом `views.py`)

    ```python
    # Схема ответа
    class $ИмяСхемы$(Schema):
    	"""Документация схемы"""

    	$АтриубтСхемы$: int = Field(..., title="Заголовок атрибута", description="Полное Описание атрибута")

    	class Config:
    		# Понятное название схемы
    		title = "Название Схемы"

    	#Валидация атрибута
    	@validator("$АтриубтСхемы$")
    	def $АтриубтСхемы$(cls, var) -> str:
    		if len(var) <= 5:
    			raise ValueError(f"Атриубт `{$АтриубтСхемы$.__name__}` не прошел валидацию")
    		return var

    # http://127.0.0.1:8090/api/ИмяПути/ДинамическийURL
    @api.<HttpМетод>('/<ИмяПути>/{$ДинамическийURL$}', summary="Краткое описание", response=$ИмяСхемы$)
    def add_basket(request: WSGIRequest, $ДинамическийURL$: int):
    	"""Полное описание API метода"""
    	return $ИмяСхемы$(<АтриубтСхемы>=<Значение>)
    ```

    Классы

    ```python
    # Схема ответа
    class BasketSh(Schema):
    	id_product: int
    	count: int = 1
    	name: str


    # Объединяющей класс
    class Basket:
    	@staticmethod
    	@api.<HttpМетод>('/<ИмяПути>', response=BasketSh, summary="Краткое описание метода")
    	def add_product(request):
    		return BasketSh(id_product=1, count=2, name="Имя")  # get_object_or_404(self.tasks, id=task_id)

    	@staticmethod
    	@api.<HttpМетод>('/<ИмяПути>', response=BasketSh, summary="Краткое описание метода")
    	def delete_product(request):
    		return BasketSh(id_product=1, count=2, name="Имя")  # get_object_or_404(self.tasks, id=task_id)

    ```

    ![Фото описания](_attachments/86ca1f593ba073137a79478ff4c8009f.png)

1. Подключить этот файл в маршрутизацию `URL`
   `Nmae_proj->urls.py`

    ```python
    from django.conf.urls.i18n import i18n_patterns
    from django.conf.urls.static import static
    from django.contrib import admin
    from django.urls import path, include

    from test.api import api

    urlpatterns = [
    		path('admin/', admin.site.urls),
    		...
    		path('api/', api.urls)
    ]
    ```

---

## Схема - Создание схемы из модели `ORM`

Класс `Schema` основана на классе `BaseModel` из `Pydantic`, поэтому он имеет все возможности `Pydantic` [Pydantic](../../Уроки/Блок6%20-%20Сети.md#Pydantic)]

### Схемы не связанные с модель БД

- Чтобы показать, какую схемы мы вернем из метода, ей следуют указать в атрибуте `@api.post('/<URL>',response=<ИмяСхемы>)`
- Можно указаывать занчени япо умолчанию у атриубтов схемы

---

- Для `GET` запросов использовать `Query(...)`

    ```python
    from ninja import NinjaAPI, Schema, Query

    # Схема ответа
    class Basket(Schema):
    	"""Отображаемая документация 1"""
    	id_product: int
    	count: int
    	name: str

    # Схема запроса
    class User(Schema):
    	"""Отображаемая документация 2"""
    	user_id: int
    	token: str

    	# Валидация
    	@validator("token")
    	def main_validator(cls, var: str) -> str:
    		if len(var) <= 5:
    			raise ValueError("key old dont Validator")
    		return var


    @api.post('/basket',response=Basket)
    def basket(request, user_id: User = Query(...))):
    	# Благодаря Query(...) у нас есть формы для заполения полей
    	return Basket(id_product=1, count=2, name="Имя")
    ```

- Для запросов с динамическим `URL` использовать `Path(...)`

    ```python
    from ninja import NinjaAPI, Schema, Path

    class <ИмяСхемы>(Schema):
    	year: int
    	month: int
    	day: int


    @api.get("/events/{year}/{month}/{day}")
    def events(request, date: <ИмяСхемы> = Path(...)):
    	...
    	return {"date": date}
    ```

- Для `POST` запросов использовать `Form(...)` и любых других, позволяет передавать дополнительные настройки для атрибута, например краткое(`title`) и полное описание(`description`).

    ```python
    from ninja import NinjaAPI, Schema, Form


    class <ИмяСхемы>(Schema):
    	year: int
    	month: int
    	day: int


    @api.post("/<ИмяПути>")
    def events(request, date: <ИмяСхемы> = Form(...)):
    	...
    	return {"date": date}
    ```

- Для файлов `File(...)`

    ```python
    from ninja import NinjaAPI, File, UploadedFile


    @api.post('/<ИмяПути>')
    def file(request, file: UploadedFile = File(...)):
    	...
    	return {"read": file.read().decode("utf-8")}
    ```

    Загрузить несколько файлов

    ```python
    from ninja import NinjaAPI, File, UploadedFile


    @api.post('/<ИмяПути>')
    def <ИмяФункции>(request, files:List[UploadedFile] = File(...)):
    	...
    	return [tmp](<{"read": [_file.read().decode("utf-8") for _file in files]}>)
    ```

- Отдавать разные схемы в зависимости от статуса ответа

    ```python
    class Token(Schema):
    	token: str
    	expires: date

    class Message(Schema):
    	message: str


    @api.post('/login', response={200: Token, 401: Message, 402: Message})
    def login(request, payload: Auth):
    	if auth_not_valid:
    		return 401, {'message': 'Unauthorized'}
    	if negative_balance:
    		return 402, {'message': 'Insufficient balance amount. Please proceed to a payment page.'}
    	return 200, {'token': xxx, ...}
    ```

    Для статусов которые начинаются на одну и туже цифру есть специальный тип `codes_4xx`

    ```python
    from ninja.responses import codes_4xx


    @api.post('/login', response={200: Token, codes_4xx: Message})
    def login(request, payload: Auth):
    	if auth_not_valid:
    		return 401, {'message': 'Unauthorized'}
    	if negative_balance:
    		return 402, {'message': 'Insufficient balance amount. Please proceed to a payment page.'}
    	return 200, {'token': xxx, ...}
    ```

### Связанные с моделью

- Для того чтобы использовать атрибуты, из существующей модели - используйте вложенный класс `Config`. И для того чтобы у нас были формы заполнения данных, используйте `Query(...)`

    ```python
    from ninja import NinjaAPI, Schema, Query
    from ninja.orm import create_schema


    class <ИмяСхемы>(ModelSchema):
    	class Config:
    		model = <ИмяМоделиБд>
    		# Столбцы которые разрешено использовать
    		model_fields = ['<Столбце1>',  '<Столбце2>',]


    @api.post('/<ИмяПути>')
    def <ИмяФункции>(request, <ИмяАргумента>: <ИмяСхемы>  = Query(...)):
    	...
    	return ...
    ```

- Если нужно отобразить связанные данные из двух таблиц. То создайте две схемы.

    ```python
    from django.db import models

    class Task(models.Model):
    	title = models.CharField(max_length=200)
    	is_completed = models.BooleanField(default=False)
    	owner = models.ForeignKey("auth.User", null=True, blank=True)
    ```

    ```python
    from typing import List
    from ninja import Schema

    class UserSchema(Schema):
    	id: int
    	first_name: str
    	last_name: str

    class TaskSchema(Schema):
    	id: int
    	title: str
    	is_completed: bool
    	owner: UserSchema = None  # ! None - to mark it as optional


    @api.get("/tasks", response=List[TaskSchema])
    def tasks(request):
    	queryset = Task.objects.all()
    	return list(queryset)
    ```

- Для файлов(ссылок) в БД

    ```python
    class Picture(models.Model):
    	title = models.CharField(max_length=100)
    	image = models.ImageField(upload_to='images')
    ```

    ```python
    class PictureSchema(Schema):
    	title: str
    	image: str
    ```

## Версионирование

[+](https://django-ninja.rest-framework.com/tutorial/versioning/)

`api_v1.py`

```python
from ninja import NinjaAPI


api = NinjaAPI(version='1.0.0')

@api.get('/hello')
def hello(request):
    return {'message': 'Hello from V1'}
```

`api_v2.py`

```python
from ninja import NinjaAPI


api = NinjaAPI(version='2.0.0')

@api.get('/hello')
def hello(request):
    return {'message': 'Hello from V2'}
```

`urls.py`

```python
from api_v1 import api as api_v1
from api_v2 import api as api_v2


urlpatterns = [
    ...
    path('api/v1/', api_v1.urls),
    path('api/v2/', api_v2.urls),
]
```

## Проверка аутентификации пользователя

- Если пользователь аутентифицирован он получит доступ к методу. [+](https://django-ninja.rest-framework.com/tutorial/authentication/)

    ```python
    from ninja.security import django_auth

    api = NinjaAPI(csrf=True)

    @api.get("/pets", auth=django_auth)
    def pets(request):
    	return f"Authenticated user {request.auth}"
    ```

- Аутентификация с помощью токена.

    ```python
    class AuthBearer(HttpBearer):
    	def authenticate(self, request, token):
    		if token == "supersecret":
    			return token


    @api.get("/bearer", auth=AuthBearer())
    def bearer(request):
    	return {"token": request.auth}
    ```

    ![Аунтификация спомощью токена.](_attachments/fb41dc98297bcbec125520c16773ad30.png)

## Распределение логики `Api` по приложениям

Для того чтобы можно было подключать `Api` из разных приложений используют классы `Router`. После этого эти объект добавляются в главный файл `Nmae_proj->api.py` с помощью метода ` api.add_router('/ИмяПуть/', <ОбьъктRouter>)` [+](https://django-ninja.rest-framework.com/tutorial/routers/)

- `Proj`
- `NameApp`

    - `api.py`

        ```python
        from ninja import Router

        # Создаем объект для импорта. ИмяТега это заголовок для `API`
        router = Router(tags=["<ИмяТега>"])


        # Пишем любой `API`
        @router.get('/{event_id}')
        def event_details(request, event_id: int):
        	event = 1
        	return {"title": event, "details": event + 1}
        ```

- `NameProj`

    - `api.py`

        ```python
        from ninja import NinjaAPI
        from NameApp.api import router

        api = NinjaAPI(version='1.0', csrf=True)

        # Доболвяем путь к `API` из другого приложения
        api.add_router('/evenv/', router)
        ```
