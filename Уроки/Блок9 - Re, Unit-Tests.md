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
| `Match`.start() -> `int` | Начало найденного слова   |
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

# Тестирование

Самый главный смысл тестирование кода, заключается в проверки работоспособности программы при её частом изменении. Если ваша программа ни когда не измениться, то вам не нужны тесты, вы можете один раз самостоятельно протестировать программу и на этом тестирование закончиться, вам не нужно писать для этого тестов, но вряд ли существуют такие программы которые ни когда не изменяются. Все успешные программы развиваются и эволюционируют, их исходный код постоянно изменяется и дорабатывается, и в этом случае каждый раз производить тестирование в ручную это большая затрата времени, в итоги когда ваша программа будет настолько большой, что изменение одно части программы, не известно как повлияют на другую часть этой программы. Для этого и нужны тесты - чтобы удостовериться, что ваша программа работает после доработок и изменений, по крайне мере она выполняет те требования которые проверяются в тестирование.

- Хорошей привычкой в тестирование является фиксация сценариев при которых произошли баги в программе.
- В тестирование программы не нужно заботиться о производительности.

## `UnitTest`

Смысл `unit-test` заключается в тестирование каждой функции и метода, по отдельности. Также `unit-test` используют для воссоздания сценариев работы. [Документация UnitTest](https://docs.python.org/3/library/unittest.html)

---

Особенности:

- Обычно все тесты программ хранят в папке `test`. Это удобно, потому что тогда можно запустить все тесты сразу (последовательно).
- Имя файлов, имя методов, имя директории = должны начинаться со слова `test`.
- Создать сценарий запуска всех тестов в папке. Выберете общую папку с тестами `test`. ![Создать сценарий запуска всех тестов в папке](_attachments/Pasted%20image%2020211125134736.png)

---

- Проект
    - test (Общая папка с тестами)
        - test_name1.py
        - test_name2.py
    - main.py

```python
import unittest
class Test_<Name>(unittest.TestCase):

	def setUp(self): # Выполнятся перед вызовом каждого метода
		...

	def test_<NameMethed>(self): # Все методы должны начинатся с слова `test_`
		...

	def tearDown(self): # Выполнятся после **успешного** выполенния каждого теста
		...

	def __del__(self): # Деструктор класса
		...

if __name__ == '__main__':
	unittest.main() # Запустить все тесты
```

---

| Метод                                           | Пример из `Python`                                                                              |
| ----------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| assertEqual(a, b)                               | a == b                                                                                          |
| assertNotEqual(a, b)                            | a != b                                                                                          |
| assertTrue(x)                                   | bool(x) is True                                                                                 |
| assertFalse(x)                                  | bool(x) is False                                                                                |
| assertIs(a, b)                                  | a is b                                                                                          |
| assertIsNot(a, b)                               | a is not b                                                                                      |
| assertIsNone(x)                                 | x is None                                                                                       |
| assertIsNotNone(x)                              | x is not None                                                                                   |
| assertIn(a, b)                                  | a in b                                                                                          |
| assertNotIn(a, b)                               | a not in b                                                                                      |
| assertIsInstance(a, b)                          | isinstance(a, b)                                                                                |
| assertNotIsInstance(a, b)                       | not isinstance(a, b)                                                                            |
| ---                                             | ---                                                                                             |
| assertRaises(`exc, fun, *args, **kwds`)         | fun(`*args, **kwds`) порождает исключение exc                                                   |
| assertRaisesRegex(`exc, r, fun, *args, **kwds`) | fun(`*args, **kwds`) порождает исключение exc и сообщение соответствует регулярному выражению r |
| assertWarns(`warn, fun, *args, **kwds`)         | fun(`*args, **kwds`) порождает предупреждение                                                   |
| assertWarnsRegex(`warn, r, fun, *args, **kwds`) | fun(`*args, **kwds`) порождает предупреждение и сообщение соответствует регулярному выражению r |
| ---                                             | ---                                                                                             |
| assertAlmostEqual(a, b)                         | round(a-b, 7) == 0                                                                              |
| assertNotAlmostEqual(a, b)                      | round(a-b, 7) != 0                                                                              |
| assertGreater(a, b)                             | a > b                                                                                           |
| assertGreaterEqual(a, b)                        | a >= b                                                                                          |
| assertLess(a, b)                                | a < b                                                                                           |
| assertLessEqual(a, b)                           | a <= b                                                                                          |
| assertRegex(s, r)                               | r.search(s)                                                                                     |
| assertNotRegex(s, r)                            | not r.search(s)                                                                                 |
| assertCountEqual(a, b)                          | a и b содержат те же элементы в одинаковых количествах, но порядок не важен                     |
| ---                                             | ---                                                                                             |
| assertMultiLineEqual(a, b)                      | строки (strings)                                                                                |
| assertSequenceEqual(a, b)                       | последовательности (sequences)                                                                  |
| assertListEqual(a, b)                           | списки (lists)                                                                                  |
| assertTupleEqual(a, b)                          | кортежи (tuplse)                                                                                |
| assertSetEqual(a, b)                            | множества или неизменяемые множества (frozensets)                                               |
| assertDictEqual(a, b)                           | словари (dicts)                                                                                 |

---

Декораторы `UnitTest`

- `@unittest.skip("<Уведомление>")` = Пропустить метод
- `@unittest.skipIf(<Условие>, "<Уведомление>")` = Пропустить метод если условие верно
- `@unittest.skipUnless(<Условие>, "<Уведомление>")` = Пропустить метод если условие НЕ верно

---

Проверка того что должно возникнуть исключение `<ИмяОшибки>` в теле `with`

```python
with self.assertRaises(<ИмяОшибки>):
	... # Except == True

```

---

## `DocTest`

Смысл `docttest` завключается в тетсирование небольших независемых функция и методов.

[Документация DocTest](https://docs.python.org/3/library/doctest.html)

```python
import doctest


def sum(a, b):
	"""
	Sum
	:param a:
	:param b:

	>>> sum(1,2)
	3
	>>> sum(5,5)
	11
	"""
	return a + b


if __name__ == '__main__':
	doctest.testmod()
```

```bush
**********************************************************************
File "/home/denis/.config/JetBrains/PyCharm2020.1/scratches/scratch_10.py", line 13, in __main__.sum
Failed example:
    sum(5,5)
Expected:
    11
Got:
    10
**********************************************************************
1 items had failures:
   1 of   2 in __main__.sum
***Test Failed*** 1 failures.
```

Добавить `DocTest` в `UnitTest`

```python
import unittest

###
import doctest
import <ИмяМодуля>

def load_tests(loader, tests, ignore):
    tests.addTests(doctest.DocTestSuite(<ИмяМодуля>))
    return tests
###

class Test_<Name>(unittest.TestCase):

	def setUp(self): # Выполнятся перед вызовом каждого метода
		...

	def test_<NameMethed>(self):
		...

	def tearDown(self): # Выполнятся после успешного выполенния каждого теста
		...

if __name__ == '__main__':
	unittest.main()
```
