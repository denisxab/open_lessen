# Настройка удобных шаблонов для `Django`

## Настройка шаблонов Html

![Настройка шаблонов Html](../_attachments/16cf18964cd9150fc6bd8d26e05ea41d.png)

---

### Простое

tag = Обратится к тегу

```bush
{% $NameTage$ %}
```

var = Обратится к переменной

```bush
{{ $name$ }}
```

for = Создать цикл

```bush
{% for $item$ in $data$ %}
$END$
{% endfor %}
```

if = Условный оператор

```bush
{% if $one$ $operand$ $two$}
$END$
{% endif %}
```

elif = Условный оператор

```bush
{% elif $Operator$ %}
$END$
```

else = Условный оператор

```bush
{% else %}
$END$
```

---

### Против дубликатов

incl = Вставить `html` контент из другого файла

```bush
{% include  "$app$/$name_file$.html"%}
```

ext = Расширить шаблон

```bush
{% extends "$app$/$name_file$.html" %}
$END$
```

block = Создать блок

```bush
{% block $name_block$ %}
$END$
{% endblock $name_block$ %}
```

---

### Статические файлы

laod = Подключить тег для статических файлов

```bush
{% load static %}
```

licss = Подключить статический `css` файл

```bush
<link href="{% static '$app$/css/$name$.css' %}" rel="stylesheet" type="text/css">
```

img = Подключить фото `img`

```bush
<img src="{% static '$app$/img/$phote$.png' %}" alt="$phote$">
```

---

### Ссылки

aurl = Создать ссылку

```bush
<a href="{% url '$name_url$' %}">$END$</a>
```

url = Обратиться к `url`

```bush
{% url '$URL$' %}
```

---

### Другие

cache = Создать кеш

```bush
{% cache $TIME$ name %}
$END$
{% endcache %}
```

### Форма

form = Создать форму для текста

```
<form method="$Method$" name="$NameForm$"
      action=""
      accept-charset="UTF-8"
      autocomplete="on"
      target="_self"
      enctype="application/x-www-form-urlencoded"> {% csrf_token %}

$END$

</form>
```

```python
<label>
    $Discription$<br>
    <input type="$TypeInput$" name="$NameInput$">
</label><br>
```

---

## Настройка шаблонов `python`

defr = Создать функцию представления

```bush
def $name$ (request:WSGIRequest):
    $END$

    context = {

    }
    return render(request,
                  template_name = "&app&/$name$.html",
                  context = context,)
```

path = Создать обработчик `URL`

```python
path('$path$/', $fan$, name="$path$"),
```

model = Создать модель БД

```python
class $NameDB$(models.Model):

    def __str__(self):
        """
        Для отображение в текстовом виде для админ панели, и консоли.
        """
        return str(self.pk)

    def get_absolute_url(self):
        """
        Для получения ссылки записи в Html и отображение в админ панели.
        """
        # return reverse("#", kwargs={"": self.pk})

    class Meta:
        """
        Вспомогательный класс для админ панели.
        """
        verbose_name = "$NameDB$"  # Имя таблицы в единственном числе
        verbose_name_plural = "$NameDB$s"  # Имя таблицы во множественном числе
        ordering = ["pk", ]  # Сортировать записи по указанным столбцам (можно указывать несколько столбцов)
```

dbform = Создать форму связанную с моделью БД

```python
class $NameModel$Form(forms.ModelForm):

    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)

        # Пример установки параметров поля. Во время создания формы
        # self.fields['ИмяПоля'].Атрибут1 = "Значение1"

    class Meta:
        """
        Класс отвечает за логику связи модели БД с формой.
        """

        # Установить связь с моделью БД
        model = $NameModel$

        # Какие поля нужно отобразить в форме
        fields = [
        $NmaeRows$
        ]

        # Какие поля нужно исключить из формы
        # exclude = []

        # Если вам нужно задать атрибуты (class) для html формы, то сделайте это здесь.
        widgets = {
            # "ИмяПоля": forms.<СпецКлассФормы>(attrs={"<Атрибут1>": Значение1, "Атрибут2": Значение2}),
        }

    @staticmethod
    def save_from_form(cls, request: WSGIRequest, true_method="POST") -> Tuple[bool, Any]:
        """
        Метод для верификации и сохранения данных, из формы, в БД. Вызывать вручную.
        """
        """
        def index(request:WSGIRequest):
            form = $NameModel$.create_form(request)
            context = {"form": form[1], ... }
            return render(request, template_name="Name_app/Name.html", context=context, )
        """
       if request.method == true_method:  # Проверим метод запроса с необходимым методом
            form = cls(request.POST)  # Если метод подходящий, то создаем объект формы
            if form.is_valid():  # Проверим валидность данных из формы
                form.save()  # Если данные корректны, то сохранить данные в БД
                return True, ""  # Возвращаем заглушку
            return False, form  # Если валидация не пройдена, то вернем форму с сообщениями об ошибках
        else:
            return False, cls()  # Если метод не подходящий, то вернем форму для html шаблона
```

### Классы предстовления

[Все шаблоны в заголовке](#Классы/представления)

## Настройка шаблона приложения

Рекомендую сделать следующую структуру шаблона приложения

---

Приложение

`/venv/lib/python3.9/site-packages/django/conf/app_template`

- `app_template`

    - `migrations`
        - `__init__.py-tpl`
    - `templates` (+)

        - `app_name` (+)

            - `index_main.html`(+)

                ```html
                <!DOCTYPE html>
                <html lang="ru">
                <head>
                	<meta charset="UTF-8">
                	<title>Title</title>
                </head>
                <body>

                </body>
                </html>
                ```

    - `static` (+)
        - `app_name` (+)
            - `css` (+)
            - `html` (+)
            - `js` (+)
            - `img` (+)
    - `__init__.py-tpl`
    - `admin.py-tpl`
    - `apps.py-tpl`
    - `models.py-tpl`
    - `tests.py-tpl`
    - `views.py-tpl`
    - `urls.py-tpl` (+)

        ```python
        from django.urls import path

        from {{ app_name }}.views import index_main

        urlpatterns = [
        	path('', index_main, name="index_main"),
        ]
        ```

---
