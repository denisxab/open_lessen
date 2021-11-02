# Регулярные выражения

Регулярными выражениями называются шаблоны, которые используются для поиска соответствующего фрагмента текста и сопоставления символов.

По сути, наш шаблон — это набор символов, который проверяет строку на соответствие заданному правилу. Давайте разберёмся, как это работает.

## Таблицы

Сокращения

| Знак | Описание                       | Аналог          |
| ---- | ------------------------------ | --------------- |
| `\d` | Любая цифра                    | `[0-9]`         |
| `\D` | Любая **не** цифра             | `[^0-9]`        |
| `\w` | Любая буква,цифра,`_`          | `[A-Za-z0-9_]`  |
| `\W` | Любая **не** буква,цифра,`_`   | `[^A-Za-z0-9_]` |
| `\s` | Любой пробельный символ        | `[\n\t ]`       |
| `\S` | Любой **не** пробельный символ | `[^\n\t ]`      |

---

| Символ                 | Обозначение                                          |
| ---------------------- | ---------------------------------------------------- |
| `.`                    | Один любой символ, кроме `\n`                        |
| `?`                    | 0 или 1 вхождение шаблона слева                      |
| `+`                    | 1 и более вхождений шаблона слева                    |
| `*`                    | 0 и более вхождений шаблона слева                    |
| `[ ]`                  | Один из символов в скобках                           |
| `[^ ]`                 | Любой символ, кроме тех, что в скобках               |
| `\`                    | Экранирование специальных символов                   |
| `^`                    | Строка начинается с ...                              |
| `$`                    | Строка заканчивается с ...                           |
| `{ <start> , <stop> }` | От `start` до `stop` вхождений                       |
| `a │ b`                | Или `a` или `b` (Это можно повторять сколько угодно) |
| `( )`                  | Сгруппировать результат                              |

---

Функции библиотеки `re`

| Функция                                                        | Описание                                                                                                       |
| -------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| re.match(`<Шаблон>, <Текст>`) ->`Match`                        | Ищет соответствие шаблона в начале текста                                                                      |
| re.search(`<Шаблон>, <Текст>`) ->`Match`                       | Ищет по всей строке, но возвращает только первое найденное совпадение.                                         |
| re.findall(`<Шаблон>, <Текст>`) ->`list`                       | Возвращает список всех найденных совпадений в тексте.                                                          |
| re.split(`<Шаблон>, <Текст>, <МаксимумРазделений=0>`) ->`list` | Эта функция разделяет строку по заданному шаблону.                                                             |
| re.sub(`<Шаблон>, <НаЧтоЗаменить> ,<Текст>`) -> `str`          | Ищет шаблон в строке и заменяет его на указанную подстроку. Если шаблон не найден, строка остается неизменной. |

---

Операции с результатом

| Метод                    | Описание                  |
| ------------------------ | ------------------------- |
| `Match`.group() -> `str` | Получить текст результата |
| `Match`.start() -> `int`  | Начало найденного слова   |
| `Match`.end() -> `int`   | Конец найденного слова    |
|                          |                           |

## re.match()

---

Ищет соответствие шаблона в начале текста

---

Простой пример

```python
import re


def test_match():
	data_arr = "kosta_proger@google.com"

	pattern = r"[a-z_]+"
	res = re.match(pattern, data_arr)

	print(res)
	if res:
		print(res.group())  # kosta_proger


if __name__ == '__main__':
	test_match()
```

Реальный пример

```python
import re


def test_match(arr_employee_: list[str]) -> list[str]:
	pattern = "ОAО Роги"

	res: list[str] = []
	for empl_ in arr_employee_:
		tmp = re.match(pattern, empl_)
		if tmp:
			res.append(empl_)
	return res


if __name__ == '__main__':
	arr_employee = [
			"ОAО Роги, Организационно-распорядительные, финансово-отчетные, кадровые ",
			"ОAО Роги, Приказы, договоры, инструкции, служебные записки, протоколы и т.д.",
			"ОAО Копыто, Индивидуальные, примерные, типовые",
			]

	res = test_match(arr_employee)
	print(res) # ['ОAО Роги, Организационно-распорядительные, финансово-отчетные, кадровые ', 'ОAО Роги, Приказы, договоры, инструкции, служебные записки, протоколы и т.д.']
```

## re.search()

---

Ищет по всей строке, но возвращает только первое найденное совпадение.

---

Реальный пример

```python
import re


def test_match(arr_employee_: list[tuple[str, str]]) -> list[tuple[str, str]]:
	pattern = "[Pp]ython"

	res: list[tuple[str, str]] = []
	for empl_ in arr_employee_:
		tmp = re.search(pattern, empl_[1])
		if tmp:
			res.append(empl_)
	return res


if __name__ == '__main__':
	arr_employee = [
			("Иван", "Я специалист по компьютерным наукам, знаю Python"),
			("Семен", "Я специалист по компьютерным наукам, знаю python"),
			("Костя", "Я специалист по компьютерным наукам, знаю Java"),
			]

	res = 	test_match(arr_employee)
	print(res) # [('Иван', 'Я специалист по компьютерным наукам, знаю Python'), ('Семен', 'Я специалист по компьютерным наукам, знаю python')]
```

## re.findall()

---

Возвращает список всех найденных совпадений в тексте.

---

```python
import re


def test_match(arr_employee_: list[str]) -> list[str]:
	pattern = "ОAО Роги"

	res: list[str] = []
	for empl_ in arr_employee_:
		tmp = re.findall(pattern, empl_)
		if tmp:
			res.append(tmp)
	return res


if __name__ == '__main__':
	arr_employee = [
			"ОAО Роги, Организационно-распорядительные, ОAО Роги, финансово-отчетные, кадровые ",
			"ОAО Роги, Приказы, договоры, инструкции, служебные записки, протоколы и т.д.",
			"ОAО Копыто, Индивидуальные, примерные, типовые",
			]

	res = test_match(arr_employee)
	print(res) # [['ОAО Роги', 'ОAО Роги'], ['ОAО Роги']]
```

## re.split()

---

Эта функция разделяет строку по заданному шаблону.

---

```python
import re
from pprint import pprint


def test_match(arr_employee_: list[str]) -> list:
	pattern = "[@.]"

	res = []
	for empl_ in arr_employee_:
		tmp = re.split(pattern, empl_)
		if tmp:
			res.append(tmp)
	return res


if __name__ == '__main__':
	arr_mail = [
			"employee1@mail.ru",
			"employee2@tut.by",
			"employee3@yandex.ru",
			"employee1@mydomain.by",
			"employee2@mydomain.by",
			"employee3@mydomain.by",
			]

	res = test_match(arr_mail)
	pprint(res)

"""
[['employee1', 'mail', 'ru'],
 ['employee2', 'tut', 'by'],
 ['employee3', 'yandex', 'ru'],
 ['employee1', 'mydomain', 'by'],
 ['employee2', 'mydomain', 'by'],
 ['employee3', 'mydomain', 'by']]
"""
```

## re.sub()

---

Ищет шаблон в строке и заменяет его на указанную подстроку. Если шаблон не найден, строка остается неизменной.

---

```python
import re
from pprint import pprint


def test_match(arr_employee_: list[str]) -> list:
	pattern = "mail|tut|yandex|mydomain"
	repl = "google"

	res = []
	for empl_ in arr_employee_:
		tmp = re.sub(pattern,repl,empl_)
		if tmp:
			res.append(tmp)
	return res


if __name__ == '__main__':
	arr_mail = [
			"employee1@mail.ru",
			"employee2@tut.by",
			"employee3@yandex.ru",
			"employee1@mydomain.by",
			"employee2@mydomain.by",
			"employee3@mydomain.by",
			]

	res = test_match(arr_mail)
	pprint(res)

"""
['employee1@google.ru',
 'employee2@google.by',
 'employee3@google.ru',
 'employee1@google.by',
 'employee2@google.by',
 'employee3@google.by']
"""

```

## Группировка результата

```python
import re
from pprint import pprint


def test_match(arr_employee_: list[str]) -> list:
	pattern = "([a-z0-9]+)@([a-z]+)"
	repl = "google"

	res = []
	for empl_ in arr_employee_:
		tmp = re.search(pattern, empl_)
		if tmp:
			res.append((tmp.group(1), tmp.group(2)))
	return res


if __name__ == '__main__':
	arr_mail = [
			"employee1@mail.ru",
			"employee2@tut.by",
			"employee3@yandex.ru",
			"employee1@mydomain.by",
			"employee2@mydomain.by",
			"employee3@mydomain.by",
			]

	res = test_match(arr_mail)
	pprint(res)

"""
[('employee1', 'mail'),
 ('employee2', 'tut'),
 ('employee3', 'yandex'),
 ('employee1', 'mydomain'),
 ('employee2', 'mydomain'),
 ('employee3', 'mydomain')]
"""
```

## Онлайн сервесы для регулярных выражений

[Онлайн редактор регулярных выражений](https://pythex.org/)

[Ссылка на хороший видео урок про регулярные выражения ](https://www.youtube.com/watch?v=1SWGdyVwN3E)
[Статья про регулярные выражения](https://tproger.ru/translations/regular-expression-python/)
