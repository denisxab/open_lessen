## Тесты

1. Создаем папку test где будут храниться все тесты

> ![](_attachments/814d8755ebca2f6bf835c062c4dc4140.png)
>
> Для удобства можно сразу создать шаблон теста
>
> ![](_attachments/0ca6b7212d6ce8b644908c68805d6e9d.png)

2. Настройка запуска тестов

> ![](_attachments/3c2bdc7599d6d621d6fed34595c6e486.png)

3. Добавляем конфигурацию запуска теста

> ![](_attachments/0e2f121ec05b4a3bf4098780de7a54cb.png)

4. Тут можно указать как папку с тестами так и отдельный файл

![](_attachments/586d9dc01cc2d1120c98159debff12df.png)

5. Потом просто выбираем тесты который нужно провести .Советую создать конфигурацию запуска для всех файлов

![](_attachments/5e2a2cc34f4f7f686b4db85304415712.png)

# Горячие клавиши

## Редактирование кода

- Перемещать строки местами

```cmd
Ctrl+Shift+Вверх или Ctrl+Shift+Вниз.
```

- Выделить участка код в строке

```cmd
Ctr+W или Ctr+Shift+W
```

- Дублировать строку

```cmd
Ctr+D
```

- Удалить строку

```cmd
Ctr+Y
```

- Быстро перемещаться между методами в редакторе.

```cmd
Alt+Up и Alt+Down
```

- Поставить метки в коде по которым можно потом перемещаться

```cmd
Ctrl+Shit+[1-9] – поставить метку
Ctr +[1-9] – переместиться к метки

```

- Вернуться к предыдущему месту курсора

```cmd
Ctrl + Shift + Backspace
```

- Перемещение по последним курсорам

```cmd
Ctrl+Alt+Left // Ctrl+Alt+Right
```

- Вынести выделенное в отделанную переменную

```cmd
Ctrl+Alt+V
```

- Выделить несколько строк сразу

```cmd
Alt +Shift + выделение мышью
```

- Перемещать курсор по краям скобок

```cmd
Ctr+Shift+M
```

- Переместить курсор на следящую строку

```cmd
Shift+Enter
```

- Завершить строку и Переместить курсор на следящую строку

```bush
Cttl + Shit + Enter
```

- Однострочных комментариев (// ...)

```cmd
Ctrl + /:
```

- Комментариев блока (/_..._/)

```cmd
Ctrl + Shift + /:
```

- Навигация между файлами

```cmd
Alt+<- || Alt +->
```

- Форматирование кода

```cmd
Ctr + Alt + L
```

- Посмотреть автодополнения только к префиксам и шаблонам

```bush
Ctrl + J
```

- Прокрутка предложений [+](https://www.jetbrains.com/help/idea/auto-completing-code.html#expand-a-string-at-caret-to-an-existing-word)

```bush
Alt + / # Вперед
Alt + Shift + / # Назад
```

- Генерировать случайные слова

```bush
lorem<ЧислоСлов>
```

## Подсказки в коде

- Нажмите Ctrl + Shift + I (Просмотр | Быстрое определение), чтобы просмотреть определение или содержимое символа в каретке, не открывая его в отдельной вкладке редактора.

```cmd
Ctrl + Shift + I
```

- Если курсор находится между круглыми скобками при вызове метода, нажмите чтобы открыть список допустимых параметров.

```cmd
Ctrl + P,
```

- Всплывающее окно документации

```cmd
Ctr+Q
```

- Места в коде где используется эта переменная или функция

```cmd
Ctrl + B
```

- Вызвать автодополнение и подсказки

```cmd
Ctrl + Space,
```

- Посмотреть список с метами, где используется переменная

```cmd
Зажать Ctr и нажать мышью на переменную(функцию)
```

- Пример решение выделенной проблемы(ошибки)

```cmd
Alt+enter
```

## Поиск

- Поиск в файле

```cmd
Ctr+F
```

- Поиск в файлах

```cmd
Ctr+Shift+F
```

- Поиск методов классов и переменных

```cmd
Ctr+Shift+N
```

- Pep 8 перейти к ошибке

```cmd
F2
```

- Найти места, где используется переменная или функция в большом окне

```cmd
Alt+F7
```

- Окно с закладками

```cmd
Shift+F11
```

- Переменную в области видимости

```cmd
Shift+F6
```

- Переименовать во всем проекте

```cmd
Ctr+F6
```

## Создание элементов кода

- Создать метод класса автоматически

```cmd
Ctrl+O
```

- Создать шаблон условия, обработки исключения

```cmd
Ctr+Alt+T
```

- Создать временный файл

```cmd
Ctrl + Alt + Shift + Insert,
```

## История кода

- Список недавно изменений кода

```cmd
Ctrl + Shift + E.
```

- История копирование

```cmd
Ctrl + Shift + V,
```

## Отладка

- Поставить точку остановки

```cmd
Ctrl + F8
```

# Отладка

> Посмотреть значение любой переменной в отдельном окне

```cmd
Alt+F8
```

> Посмотреть значение переменной Быстро

```cmd
Зажать Alt и навести на перерешённую
```

> Если не работает сочетание клавиш [Хитрости](#_Хитрости)

## Авто тесты

![](_attachments/4b7dce7681fae8f8b0a9e25c8b15905c.png)

> Тесты будут автоматом запускаться когда вы изменили данные

## Исправление ошибок

> Откроется удобное окно

![](_attachments/90d40f1791074463e8210792e0d5758a.png)

# Создание своих шаблонов

## Снипетты

> [Документация](https://www.jetbrains.com/help/pycharm/tutorial-creating-and-applying-live-templates-code-snippets.html#template-text)

![](_attachments/b50d03fe0e79ac081d680734a11c6211.png)

> 1. Выбираем в каком языке будет работать шаблон
>
> 2. добавляем
>
> ![](_attachments/56a1d646ec23bdb02a4f40d964e88092.png)
>
> 3.  пешим код шаблона
>
> ```python
> class $class$($object$):
> 	"""
> 	$class$
> 	"""
>
> 	__slots__ = ()
>
> 	def __init__(self,$args$):
> 		super.**init**($args$)
> 		$END$
> 		pass
>
> 	def __repr__(self):
> 		return f"{$class$}:{self.**dict**}"
>
> 	def __del__(self):
> 		pass
> ```
>
> 4.  тут писать описание шаблона
>
> 5.  тут писать имя шаблона
>
> 6.  тут можно изменять редактировать значения по умолчанию шаблона

## Значения по умолчанию

> Двойные кавычки для констант

![](_attachments/3d92f360768b3fb5b6dfb682af789ae8.png)

## Expression

[Функции ](https://i.imgur.com/Wsp15kZ.png)

## Постфикс

Стандартные
![](_attachments/a5b7be8edc6eccbbea6c5fe1b1eb351f.png)

Расширенный префиксы

```bush
Custom Postfix Templates
```

[Документация](https://github.com/xylo/intellij-postfix-templates#order-of-template-filesrules)

![](_attachments/e3c7ddd2d1f2a4e0e38ae573a5cc13c5.png)

### Добавить свой шаблон

Нужно создать отдельный файл с шаблонами
`Tools -> Custom Postfix Templates -> Edit Templates of ...`

### Правила составления шаблона

```bush
.NAME: decription

	MATCHING_TYPE  →  TEMPLATE_CODE
```

`.NAME` = Название постфикса
`decription` = Описание постфикса

---

`MATCHING_TYPE` = Для какого типа применять указанный шаблон

[Все типы](https://github.com/xylo/intellij-postfix-templates#simple-template-rules)
Типы `Python`

- `ANY`- любое выражение
- `object`
- `list`
- `dict`
- `set`
- `tuple`
- `int`
- `float`
- `complex`
- `str`
- `unicode`
- `bytes`
- `bool`
- `classmethod`
- `staticmethod`
- `type`

```bush
.example : example template
    dict →   dict($expr$)
    str →   str($expr$)
    float →   float($expr$)
    int →   int($expr$)
    list →   list($expr$)
    tuple →   tuple($expr$)
    set →     set($expr$)
    object →   object($expr$)
    ANY  →  foo($expr$)
```

---

`TEMPLATE_CODE` = Шаблон постфикса

Можно использовать любые имена в виде `$NAME_VAR$` кроме зарезервированных

- Зарезервированные имена в шаблоне
    - `$expr$`= Выражение к которому применятся постфикс
    - `$END$`= Конечное место курсора

Так же можно использовать значения по умолчанию

```python
.example : example template
    ANY  →  foo($expr$,$Name*#<Идекс>:<Выражение>:"<Значение_по_умолчанию>"$)
```

- `*`= Вставить значение по умолчанию
- `Идекс` = Номер выражения
- `Выражение` = активный макрос шаблона (редко) [ Примеры функций](https://www.jetbrains.com/help/idea/template-variables.html#predefined_functions)
- `Значение_по_умолчанию`= Значение должно быть в кавычках `"_"`!
    > Если какой-то параметр не нужен, то пропускать его как слайс `$Name$#::<"Значение по умолчанию">`

---

Интересные функции для шаблона:

`date([format])` Вставляет текущую дату
`time("H:m z")` Текущее системное время

```python
.datprint : Вывести текущую дату
    ANY →  print("$Name:date("Y-MM-d, E, H:m")$",$expr$)
```

---

`enum(<String>, ...)` Свой список дополнений для переменных

```python
.choice : Пример как создать шаблон со списком параметров
    ANY →  print("$Name:enum("1","2","3")$",$expr$)
```

---

`fileName()` Имя текущего файла с его расширением
`fileNameWithoutExtension` Имя текущего файла без его расширения
`filePath()` Полный путь до текущего файла
`fileRelativePath()` Путь относительно текущего проекта
`user()` Имя текущего пользователя
`pyFunctionName()` Имя текущей функции
`pyClassName()`Имя текущего класса

```python
.name_proj : Получить имя файла
    ANY →  print("$Name:fileName()$",$expr$)
```

---

`showParameterInfo()` Показывает окно с параметрами у вызываемой функции

```python
.show_args : Показать окно параметров функции
	ANY →  print(sum($Name:showParameterInfo()$),$expr$)
```

---

### Шаблоны

```python
.example : example template
    dict →   dict($expr$)
    str →   str($expr$)
    float →   float($expr$)
    int →   int($expr$)
    list →   list($expr$)
    tuple →   tuple($expr$)
    set →     set($expr$)
    object →   object($expr$)
    ANY  →  foo($expr$,$Name*:methodName()$)

.printD : Отправить в Debugger
    ANY → printD($expr$,$END$)

.def: Создать функци с указанным колличеством параметров
    ANY → def $expr$ ($Args$):\
        $END$
```

# Хитрости

## !

> Отключить комбинацию Alt+F8
> ![](_attachments/9a4b2cbd62204868edd4abfb940f68ae.png)

## SCSS - Sasss

[Doc scss](https://www.jetbrains.com/help/pycharm/transpiling-sass-less-and-scss-to-css.html)

---

Установка SCSS

- Установить `Node.js`

```bush
sudo pacman -S npm
```

- Установить компилятор `SCSS`

```bush
sudo npm install -g sass
```

---

Создать наблюдатель `SCSS` | `Sasss`
![](_attachments/f8e52f834698a3ce431f4e4011816686.png)

---

Настройка наблюдателя
![](_attachments/e4784827566a1361d3791d7fde15ca4d.png)

`Scorpe` Где искать изменения фала
`Program` Путь к scss. Если установили scss через `npm` то путь автоматически создастся
`Output paths to refrsh` Путь выхода компилированного файла

---

Создать файл SCSS
![](_attachments/e53653d3602a31f775a2341f809493dd.png)

---

Вот такой результат
![](_attachments/18f9ffd77cddeef92b7d5fbdb3f6dfce.png)

## Git

[Git](../Знания/Git.md##Pychram%5CGit)

## Построить диаграмму классов

[Doc](https://www.jetbrains.com/help/pycharm/class-diagram.html#manage_class_diagram)
![Приме внешнего вида](_attachments/99c6a12905bc4a020d8065a02723ceb5.png)

1. Показать/Скрыть (методы/функции)

```bush
Ctrl + Alt + U # Вызвать всплывающие окно
Shift + Ctrl + Alt + U # Отдельная вкладка
```

## Размер окна PyCharm

> Изменять размер по прокрутки мыши
> ![](_attachments/8a5d24f457e6a4d25a82417943bc25ca.png)

## Импорт | Экспорт настроек PyCharm

> ![](_attachments/50671ee89ebb7b1e73f7e4b64835b0a8.png)

## Свернуть Код

> ![](_attachments/061032869c3d0b8264d59fc311e106d8.png)

## Структура проекта

```bash
# Свернуть весь код
cntrl+shift+-
# Развернть код на достаточный уровень
cntrl+shift++
```

> ![](_attachments/7423474f5ef6f3bc8b4ef144ac5fcada.png)

## Заметки закладки

> ![](_attachments/569836177d9b49f6c93297b18ce99faa.png)

## TODO

> Чтобы создать TODO напишите
>
> ```cmd
>  #: TODO Написать напоминание
> ```
>
> ![](_attachments/2008e0ccd48e3510e8e2686b8ffa7c11.png)

## Requirements.txt

> ![](_attachments/f3eb5e52b101086c91ac8cbc128ee625.png)

## Добавить проверку орфографии

> Скачать плагин Grazie
> ![Grazie](https://i.imgur.com/8z31o8G.png)
>
> или
>
> ![](_attachments/a8ada5dc282b9ecc09f509474a58ff65.png)
>
> Качаем именно исходный код
>
> ![](_attachments/11a59ae2f815afa1e8f9a14e57cf5009.png) > ![](_attachments/03a70cd15f9a9b497eb9d976c6d3c2f8.png)
>
> **ПЕРЕЗАГРУЗИТЬ** проект!

## Экспорт настроект

> ![](_attachments/8565cab9164f939077bc75e5d8bf2aaf.png)

## Добавить `pylint`

Скачать плагин [pylint](https://plugins.jetbrains.com/plugin/11084-pylint)

![](_attachments/cfc786388a9f5eb875d49017435f5f20.png)

`C:\Users\denis\AppData\Local\Programs\Python\Python37\Scripts\pylint.exe`

## ИИ автокомплит

![](_attachments/dc03fd03d201ae43287179d0c5426d15.png)

## Всплывающее окно параметров для автокомплита

![](_attachments/dc6fdbf3fb50b6b0f73c9cd21fb10c98.png)
