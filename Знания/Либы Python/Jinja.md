# Jinja – Шаблоны строк

## Пример использования шаблона

```python
from jinja2 import Template

name = "Ales"
age = 22
tm = Template("Hello {{ name }} have you years {{age}}?")
msg = tm.render(name=name,age=age)
print(msg)
```

Также можно использовать функции и операции в конструкторе шаблона

```python
name = "Ales"
age = 22
tm = Template("Hello {{ name.upper() }} have you years {{age*2}}?")
msg = tm.render(name=name,age=age)
print(msg)
```

Использовать атрибуты и методы классов в шаблоне

```python
class Person:
	def __init__(self, name, age):
		self.name = name
		self.age = age

per = Person("Ales",22)
tm = Template("Hello {{ person.name.upper() }} have you years {{person.age}}?")
msg = tm.render(person = per)
print(msg)
```

Использовать словарь

```python
per = {"name":"Ales","age":32}
tm = Template("Hello {{ person.name.upper() }} have you years {{person.age}}?")
msg = tm.render(person = per)
print(msg)

per = {"name":"Ales","age":32}
tm = Template("Hello {{ person['name'] }} have you years {{person['age']*2}}?")
msg = tm.render(person = per)
print(msg)
```

```bush
{%%} - спецификатор шаблона
{{}} - для вставки python кода
{##} - блок комментарий
```

## Экранирование символов

```python
data = " {%raw%} {{name}} is {%endraw%}"
tm = Template(data)
msg = tm.render()
print(msg)
```

## Экранирование тегов HTML

```python
from markupsafe import escape
data = " text = <a href='http://'> Link </a> "
msg = escape(data)
print(msg)
```

или

```python
data = " text = <a href='http://'> Link </a> "
tm = Template("{{data | e}}")
msg = tm.render(data = data)
print(msg)
```

## Циклы в шаблонах

```python
link = [
{"name": "Mush", 'id': 1},
{"name": "Restr", 'id': 13},
{"name": "Sergo", 'id': 14},
{"name": "Yerst", 'id': 12},
{"name": "Goes", 'id': 1}
]

data = """
<start>
{% for ITEM in DATA -%}
<{{ITEM['name']}} = {{ITEM.id}}>
{% endfor -%}
<end>"""

tm = Template(data)
msg = tm.render(DATA=link)
print(msg)
```

> Убирает перенос строки `-%`

## Условные опреторы в шаблонах

```python
link = [
{"name": "Mush", 'id': 1},
{"name": "Restr", 'id': 13},
{"name": "Sergo", 'id': 14},
{"name": "Yerst", 'id': 12},
{"name": "Goes", 'id': 1}
]

data = """
<start>
{% for ITEM in DATA -%}
{% if ITEM.id % 2 == 0-%}
Четные id <{{ITEM['name']}} = {{ITEM.id}}>

{% elif ITEM.name[1] == 'e' -%}
Это работает elif

{% else -%}
Нечетные id <{{ITEM['name']}} = {{ITEM.id}}>

{% endif -%}
{% endfor -%}
<end>"""

tm = Template(data)
msg = tm.render(DATA=link)
print(msg)
```

## Макросы - функции

> Макрос внутри шаблона

```python
html = """
{% macro input(name,values="",size=20) -%}
<input name="{{ name }} size= {{"size"}}>
{%- endmacro %}

{{input(1)}}
{{input(2)}}
{{input(3)}}

"""
tm = Template(html)
msg = tm.render()
print(msg)
```

## Чтение html

```python
params = [
{"name": 'Nekit', "last_name": 'Retut'},
{"name": 'Djok', "last_name": 'Wesr'},
{"name": 'Telest', "last_name": 'Fsrtut'},
{"name": 'Ioin', "last_name": 'Seetut'}
]

# Загрузчик файлов html

file_html = FileSystemLoader(
searchpath=r"C:\Users\denis\PycharmProjects\sflask\yourapp") # Имя каталога(подготалога) с html файлами
env = Environment(loader=file_html)
tm = env.get_template('main.htm')
msg = tm.render( data=params)
print(msg)
```

> main.htm

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
</head>
<body>
{% for item in data -%}
<h2>{{item.name}} + {{item.last_name}}</h2>
{% endfor -%}
</body>
</html>
```

## Подключение заголовков шаблонов

```python
params = [
{"name": 'Nekit', "last_name": 'Retut'},
{"name": 'Djok', "last_name": 'Wesr'},
{"name": 'Telest', "last_name": 'Fsrtut'},
{"name": 'Ioin', "last_name": 'Seetut'}
]

# Загрузчик файлов htmlfile_html = FileSystemLoader(searchpath=r"C:\Users\denis\PycharmProjects\sflask\yourapp") # Имя каталога(подготалога) с html файлами

env = Environment(loader=file_html)
tm = env.get_template('page.html')
msg = tm.render(data=params)
print(msg)
```

> page.html

```html
{% include 'header.html' %}
{% for item in data -%}

<h2>{{item.name}} + {{item.last_name}}</h2>
{% endfor -%}
{% include "footer.html" %}
```

> header.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
</head>
<body>
```

> footer.html

```html
</body>
</html>
```

> Если нужно проигнорировать ошибку при отсутствии файла, то пешим
> `{% include 'header.html' ignore missing%}`

## Импорт шаблонов с макросами

```python
params = [
{"name": 'Nekit', "last_name": 'Retut'},
{"name": 'Djok', "last_name": 'Wesr'},
{"name": 'Telest', "last_name": 'Fsrtut'},
{"name": 'Ioin', "last_name": 'Seetut'}
]

# Загрузчик файлов html

file_html = FileSystemLoader(searchpath=r"C:\Users\denis\PycharmProjects\sflask\yourapp") # Имя каталога(подготалога) с html файлами
env = Environment(loader=file_html)
tm = env.get_template('page.html')
msg = tm.render(data=params)
print(msg)
```

> page.html

```html
{%import "macrs.html" as mc%}
{% include 'header.html' ignore missing%}
{% for item in data -%}
{{ mc.input(item.name,item.last_name) }}
{% endfor -%}
{% include "footer.html" %}
```

> macrs.html

```html
{% macro input(name, last_name) -%}

<p>{{name}} += {{last_name}}</p>
{%- endmacro %}

header.html

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
</head>
<body>
```

> footer.html

```html
</body>
</html>
```

## Блоки шаблонов

> Внимание, нужно отображать именно расширенный файл!

```python
params = [
{"name": 'Nekit', "last_name": 'Retut'},
{"name": 'Djok', "last_name": 'Wesr'},
{"name": 'Telest', "last_name": 'Fsrtut'},
{"name": 'Ioin', "last_name": 'Seetut'}
]

# Загрузчик файлов html

file_html = FileSystemLoader(searchpath=r"C:\Users\denis\PycharmProjects\sflask\yourapp") # Имя каталога(подготалога) с html файлами
env = Environment(loader=file_html)
tm = env.get_template('about.html')# Берм именно файл где написанн {% extends "main.html" %}
msg = tm.render(data=params)
print(msg)
```

> main.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>{% block title %}{% endblock %}</title> # Вставить блок
</head>
<body>
{% block content %}
{% endblock %}
</body>
</html>
```

> about.html

```html
{% extends "main.html" %} # Указать файл в который расширить
{% block title %} # Вот блок
Name company
{% endblock %}
{% block content %}

<h1>about the site</h1>
{% endblock %}
```

> Можно в самом блоки использовать блоки

```html
{% extends "main.html" %}
{% block title %}Name company{% endblock %}
{% block content %}<h1>{{ self.title() }}</h1> # Вызываем блок как функцию

<h1>about the site</h1>
{% endblock %}
```

---

> `block` Вставляется из того файла где написан `extends` !

```css
{% extends "header.html" %}
{% block TEST %}
<form action="/contact" method="post" class="form-contact">
 <p><label>Name</label><input type="text" name="username" value="" required/>
 <p><label>Email</label><input type="text" name="email" value="" required/>
 <p><label>Massage</label> <textarea name="message" cols="30" rows="10"></textarea>
 <p><input type="submit" value="Send"/>
</form>
{% endblock TEST %}
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
 <meta charset="utf-8">
</head>
<body>
<h1>12312313</h1>
{% block TEST %}
{% endblock %}
<h1>12312313</h1>
```

## Подключить CSS к HTML

```html
<link type="text/css" href="{{ url_for('static', filename='css/NAMEFILE.css') }}" rel="stylesheet"/>
```

### Импорт CSS

> test.css

```css
@import url("test2.css");
h1 { color: red}
```

> test2.css

```css
h2 {color: green}
```
