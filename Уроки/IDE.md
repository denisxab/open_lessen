# Введение

Существуют достаточно много инструментов для написания кода. И лидирующими программы для это сейчас являются

- `PyCharm`
- `VS Code`
    ![Статистика пользователй IDE](_attachments/b12d3db878b0da11c60058b334a11c7c.png)

В данном уроке мы рассмотрим профессиональную интегрированною среду разработки `PyCharm`.

- [Создание проекта и виртуальное окружение](#создание/проекта/и/виртуальное/окружение)
- [Навигация в проекте](#Навигация/в/проекте)
- [Примеры использования полезных горячих клавиш](#Примеры/использования/полезных/горячих/клавиш)
- [Использование Живых шаблонов `Live Templatse`](#Использование/Живых/шаблонов/Live/Templatse)
- [Использование постфиксов `Postfix` и плагины `Custom Postfix Templates`](#Использование/постфиксов/Postfix/и/плагины/Custom/Postfix/Templates)
- [Работа с `Git` и подключение к `GitHab`](#Работа/с/Git/и/подключение/к/GitHab)
- [Тестирование кода](#Тестирование/кода)

# Создание проекта и виртуальное окружение

`File -> New Project`

![Создание проекта](_attachments/0425bddbfe083119ab88228b6c3e4c0c.png)

1. Путь где будет находиться проект
2. Путь где будет находиться виртуальное окружение
3. Вы можете использовать уже ранее созданные виртуальное окружения
4. Шаблоны проектов для различных Фреймворков

- `Inherit global site-packages` Если вы хотите, чтобы все пакеты, установленные в глобальном Python на вашем компьютере, добавлялись в виртуальную среду, которую вы собираетесь создать.
- `Make available to all projects` Если вы хотите повторно использовать эту среду при создании интерпретаторов Python в PyCharm.

---

Если после создания проекта вам нужно поменять виртуальное окружение
вы можете это сделать.

![Если потерян ВО](_attachments/8b8602f9f68737cfb20d08de72188dc2.png)

1. Здесь вы можете добавить существующее ВО или создать новое
   ![Прмиер выбора ВО](_attachments/0feb77067eee8681ba387e97096baeb1.png)

2. Также тут можно добавить пакет из PIP репозитория
   ![](_attachments/e069ba669deed3dacc296bcf638e92dd.png)

# Навигация в проекте

Одна из главных особенностей `PyCharm` - это возможность
автоматически изменять пути импортирования модулей при
переносе модуля

![](_attachments/584318a261aebfd76e201d36631fbdf4.png)

- `New` Создать файл по шаблону
- `Copy`Копировать относительный, или абсолютный путь к файлу
- `Refactor` Переименовать, переместить, Копировать файл или
    директорию
- `Show Explorer` Показать в проводнике
- `Open in Terminal` Открывает терминал, и переходит
    к ближайшей директории в котором находиться файл.
- `Local History` Можете посмотреть историю изменения файла
- `Mark Directory as` Установить папку как `Sources Root` или `Tamplate`

Использование `Sources Root`
![](_attachments/1ac99ffe207f6e67bf458bcece52433b.png)

---

Перемещение модулей

![До](_attachments/381dc8b367c644bfdb450d61488d3abc.png)

---

![В процессе](_attachments/8e81d025feff7b59d517b78276cd8d08.png)

---

![После](_attachments/3149296ef473da6b29e830213ef68291.png)

# Примеры использования полезных горячих клавиш

## Редактирование кода

- Перемещать строки местами

```cmd
Ctrl+Shift+Вверх или Ctrl+Shift+Вниз.
```

- Выделить участка код в строке, исходя из синтаксиса языка программирования

```cmd
Ctrl+W или Ctrl+Shift+W
```

- Дублировать строку

```cmd
Ctrl+D
```

- Удалить строку

```cmd
Ctrl+Y
```

- Быстро перемещаться между методами в редакторе.

```cmd
Alt+Up и Alt+Down
```

- Поставить метки в коде по которым можно потом перемещаться "На линукс другая комбинация"

```cmd
Ctrl+Shit+[1-9] – поставить метку
Ctrl +[1-9] – переместиться к метки
```

- Вернуться к предыдущему месту курсора

```cmd
Ctrl+Shift+Backspace
```

- Перемещение по последним курсорам

```cmd
Ctrl+Alt+Left // Ctrl+Alt+Right
```

- Вынести выделенное в отделанную переменную

```cmd
Ctrl+Alt+V
Shift+Alt+V [Linux]
```

- Выделить несколько строк сразу

```cmd
Alt +Shift + выделение мышью
```

- Перемещать курсор по краям скобок

```cmd
Ctrl+Shift+M
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
Ctrl + Alt + L
Alt + Shift + L [Linux]
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

- Быстрое просмотр определения

```cmd
Ctrl + Shift + I
```

![](_attachments/323f537548fc2e157461bccc5dd31717.png)

- Список допустимых параметров.

```cmd
Ctrl+P
```

![](_attachments/7cd6eaee5cad48c7ea5b79ec30aa08d8.png)

- Всплывающее окно документации

```cmd
Ctrl+Q
```

![](_attachments/c267d02448a8ced957154c02a692ac6f.png)

- Для объявления = Показать места в коде, где используется функция.
- Для вызова = Открывает объявления функции.

```cmd
Ctrl + B
```

![](_attachments/ba7c1bef5b5d52de6efbcee0ef9b233c.png)

- Принудительно вызвать автодополнение

```cmd
Ctrl+Space,
```

- Посмотреть автодополнения только к постфиксам и шаблонам

```bush
Ctrl+J
```

![](_attachments/ac634a5278447451bb61ece31fe8b608.png)

- Пример решение проблемы

```cmd
Alt+Enter
```

![](_attachments/c2ecee63ce0784196f5f2036f4a05886.png)

## Поиск

- Поиск в текущем файле

```cmd
Ctrl+F
```

- Поиск в файлах проекта

```cmd
Ctrl+Shift+F
```

- Замена в файлах проекта

```bush
Crtl+Shift+R
```

![](_attachments/a656e11bce5787903dfc0994d16ac63c.png)

- Перейти к сведущий ошибке

```cmd
F2
```

![](_attachments/985dcd44e89cd2235a937456da9d2499.png)

## Создание элементов кода

- Создать метод класса автоматически

```cmd
Ctrl+O
```

![Прсотой варинт](_attachments/1c06ffd329a97cffaa1ebf09121ec79d.png)
![Переопределение методов](_attachments/9cfe5887b5d92cecb54e6b669c43f068.png)
![Результат переопределения](_attachments/9b58aefff11ae73c063d3b8d3c98ea43.png)

Также можно переопределять другие методы класса
![Переопределение](_attachments/038448e5ab1c993bfa065b1036e1662e.png)
![Результат](_attachments/3ce0244a37da2679f4ea86d07825ce68.png)

- Создать временный файл

```cmd
Ctrl + Alt + Shift + Insert
```

![Временный файл](_attachments/ff0d26a8ffb08d5e2dee9eb87bbfda75.png)

## История кода

- Список недавно изменений кода

```cmd
Ctrl + Shift + E.
```

![](_attachments/62278d7296fcc3c903e2dcb34a97bf7e.png)

- История копирование

```cmd
Ctrl + Shift + V,
```

![](_attachments/7cde11098c840dedd9acfabdcd266d3d.png)

## Отладка

- Поставить точку остановки

```cmd
Ctrl + F8
```

# Использование Живых шаблонов `Live Templatse`

Пример использования `Live Templatse`
![Подсказка](_attachments/b540fea1fead47a1a8a2c50d6fee953b.png)
![Результат](_attachments/a5ee534644b01f0521364123cb0813b4.png)

![](_attachments/1c88a12d3bf841be8e9e04fec4c4f2b6.png)

1. Добавить шаблон в текущую группу
2. Добавить группу
3. Горячая клавиша создания шаблона

Пример создания шаблона
![](_attachments/957b5de4ae32ef01448881fab169548d.png)

1.  Шаблонное слово
2.  Описание шаблонного слова
3.  Где будет действовать шаблон
    ![](_attachments/da93a86f2efe087d55fc15a5ecd7dcab.png)
4.  Указать значения по умолчанию, и шаблонные функции
    ![](_attachments/8217419a5202f938633f95046481d3d5.png)

    1.  Переопределенные функции [Документация](https://www.jetbrains.com/help/idea/template-variables.html#predefined_functions).
    2.  Значение по умолчанию должно быть обязательно в двойных ковычках `" "`.
    3.  Вставить значение по умолчанию.

# Использование постфиксов `Postfix` и плагины `Custom Postfix Templates`

Пример постфикса

![Пример постфикса1](_attachments/392bd065f19011c4e4c78531e532dfd8.png)
![Результат постфикса1](_attachments/bf65dd1a99bfd565695ed5495230571b.png)

---

![Пример постфикса2](_attachments/d79c4489ee0b045933ff1383af23f754.png)
![Выбрать варинт постфикса2](_attachments/327635fff672171c30382434f94976e1.png)
![Результат постфикса2](_attachments/c9e5fcc564c163ac003e947963f48ff1.png)

Стандартные постфиксы
![Стандартные постфиксы](_attachments/fe506874d11d07af3accc47b84e4554e.png)

Плагин `Custom Postfix Templates`
[Ссылка на плагин](https://plugins.jetbrains.com/plugin/9862-custom-postfix-templates)

После установки плагины нажмите чтобы быстро открыть меню плагина

```bush
Ctrl + Alt + Shift + P
```

Добавить свой постфикс
![](_attachments/8c054bd069e8ae93bb51d256b895a037.png)

Окно с пользовательскими шаблонами

```bush
Alt + Shift + P
```

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

.def : Создать функци с указанным колличеством параметров
    ANY → def $NameDef$ ($expr$):\
    $END$

.list : В list
    ANY → list($expr$)$END$

.self : self.Var
    ANY → self.$expr$ = $expr$ $END$

.type : В list
    ANY -> type($expr$)$END$

.printD : Отправить в Debugger
    ANY → printD($END$,$expr$)

```

# Работа с `Git` и подключение к `GitHab`

## Включить контроль версий

`VCS -> Enable Version Control Integration`
![Выбираем вид VCS](_attachments/b89110adf7c7ec1f74fd0e1482765844.png)

## Создать `.gitignore`

```bush
/venv
/.git
/.idea
```

## Добавляем файлы в отслеживание

![](_attachments/5c9fc8e0c678e10d78924bcdced6c290.png)

`Add to VCS`

## Создаем `commit`

![](_attachments/67889c0672f40970f1627cd5a249c9ad.png)

## Делаем `push`

![](_attachments/160a6a689986278fe2a8eaf292f3d120.png)

Создать удаленный сервер
![Создать удаленный сервер](_attachments/29f26a45fd9af7ad0b39d453825e8360.png)

# Тестирование кода

Создаем файл с тестами

```python
import unittest


class MyTestCase(unittest.TestCase):
	def test_something(self):
		self.assertEqual(True, False)


if __name__ == '__main__':
	unittest.main()

```

![](_attachments/609e8754f9bd0bb1afc9a1eccefc65e6.png)

1. `Eddit Configuration`
2. Выбираем библиотеку для тестирования

![](_attachments/e495e4a27d027a2f8acb3314685b5b5a.png)

1. Указываем путь к папке с тестами, или указываем путь на весь проект
2. Если указать весь проект, то `PyCharm` по умолчанию будет искать и запускать
   файлы начинающиеся с именем `test`

Пример правильной компоновки тестов
![](_attachments/b1c4b90dd11f3dafab2e14704105456c.png)

Не правильный пример, но так тоже будет работать
![](_attachments/cc31508de044ab547e5a0429a20806d9.png)
