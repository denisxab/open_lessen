# Введение

## Установка

Установка:

```bash
pip install click
```

---

Ресурсы:

- [Api](https://click.palletsprojects.com/en/8.0.x/api/)

---

## Определения

Термины:

- Аргумент - Позиционное значение
- Параметр - Именованное значение (мы можем указать как полное имя `--`, так и краткое `-`) (можно указать несколько одинаковых имен через `'--$Имя1$/--$Имя2$'`)
- Флаг - Истина или ложь

```python
import click


@click.command()
@click.option('path', "--path", '-p', type=click.File())
def greet(path):
    click.echo(f"Hello {path.read()}!")


if __name__ == '__main__':
    greet()

```

# Логика

## Декораторы

---

Основные декораторы:

- `@click.command()` - Главный декоратор создания команды
    - `help="Команда"` - Описание команды
- `@click.option('$Param$')` - Говорит о том что мы ожидаем параметры

    - `$Param$:str`- Имя обрабатываемого параметра, может начитаться с `--` или `-`.
    - `required:bool=True` - Обязательный для заполнения
    - `type:Any` Тип, используемый для проверки значения. Можно указат несколько доступных значений через запятую
    - `default:Any` - значение по умолчанию
    - `show_default:str|bool=None` - Подсказка о знание по умолчанию
    - `help:str` - Текст подсказка
    - `envvar:str=''` - Извлечь значение из переменных [окружения](https://click.palletsprojects.com/en/8.0.x/options/#values-from-environment-variables)
    - `multiple:bool=False` - Разрешить передавать несколько значений (разделить значения Linux `:`, Windows `;` )
    - `callback:Callback=None` - Вызвать функцию перед входом. [Обработка ошибок](https://click.palletsprojects.com/en/8.0.x/options/#callbacks-for-validation)к
    - `nargs:int=None` - Сколько значений получить из строки (-1 получить все [элементы](https://click.palletsprojects.com/en/8.0.x/arguments/#variadic-arguments))

    ***

    - `prompt:bool|str=False` - Если значение не передали в запросе, то вызвать `input()`. Если мы хотим отправить вспомогательное сообщение то вместо `True` укажите строку с сообщением.
    - `prompt_required:bool=False` - Вызвать `input` если значение в параметр не [передан](https://click.palletsprojects.com/en/8.0.x/options/#optional-value)
    - `hide_input:bool=False` - Скрыть вводимый текст.
    - `confirmation_prompt:bool|str=False`- Запросить подтверждение ввода.

    ***

    - `is_flag:bool=False` - Сделать из парамера **флаг**
    - `flag_value:Any=None` - Значение флага если он указан

- `@click.argument('$ИмяАргументаВ_Функцию$')` - Говорит о том что мы ожидаем позиционный аргумент, который будет передан в функцию по указанному имени

    - `nargs:int=None` - Сколько значений получить из строки (-1 получить все [элементы](https://click.palletsprojects.com/en/8.0.x/arguments/#variadic-arguments))

    ```python
    @click.command()
    @click.argument('infile', nargs=1)
    @click.argument('out', nargs=1)
    def copy(infile, out):
    	print(infile, "+", out)

    #╰─➤  python console.py  __pycache__/ __pycache__/helpful.cpython-310.pyc                                    1 ↵
    #__pycache__/ + __pycache__/helpful.cpython-310.pyc
    ```

---

Вспомогательные декораторы:

- `@click.password_option()`- Запросить пароль от пользователя. Функция принимает аргумент `password`
- `@click.confirmation_option(prompt='$Сообщение$')` - Запросить подтверждение действия

---

## Функции:

Получить данные из консоли

- `message = click.edit("$ТекстВ_Редактор$")` - Открыть консольный редактор. После закрытия редактора данные вернуться из функции.
    - `filename:str=None` - Путь к файлу куда сохраниться результат. В этом случае функция не вернет текст.

Отправить данные в консоль

- `click.echo("$Текст$")` - Вывести текст в консоль, аналог `print`, но имеет доп функционал. ([Раскрасить текст](https://click.palletsprojects.com/en/8.0.x/utils/#ansi-colors))
- `click.echo_via_pager("$Текст$")` - Показать текст в виде прокручивающегося окна (по типу `less`) (чтобы выйти из, него нажмите `q`)

- `click.progressbar($Итератор$)` - Прогресс бар

    - `label:str=None` Описание прогресса

        ```python
        with click.progressbar([1, 2, 3],
        					   label="Прогресс", ) as bar:
        	for x in bar:
        		print(f"sleep({x})...")
        		time.sleep(x)
        ```

        ```txt
        Прогресс  [------------------------------------]    0%sleep(1)...
        Прогресс  [############------------------------]   33%  00:00:02sleep(2)...
        Прогресс  [########################------------]   66%  00:00:01sleep(3)...
        Прогресс  [####################################]  100%
        ```

Утилиты

- `click.clear()` - Отчистить экран консоли

- `click.pause('$Сообщение$')` - Поставить терминал на паузу, пока не нажмут пробел

- `click.launch($Url$)` - Открыть путь или ссылку в приложение по умолчанию

    - `locate:bool=None` Указать что путь находиться локально.

---

## Группировка - документирование

- `@click.group()` - Создать группу
    - `help="Программа"` - Описание группы
- `$GROOP$.add_command($Команда$, <"$ИмяВ_Консоле$">)` - Добавить команду в группу

```python
import click


# Создаем группу
@click.group()
def cli():
    """Описание группы"""
    pass


@click.command("nameone", short_help="Краткое описание команды 1")  # Указываем внешнее имя для команды
@click.option('path', "--path", '-p', type=str, help="Описание флага 1")
@click.argument('mode')
def greet_p(path, mode):
    """Полное описание команды 1"""
    click.echo(f"Hello {path}, {mode}")


@click.command("nmaetwo")  # Указываем внешнее имя для команды
@click.option('file', "--file", '-f', type=str, help="Описание флага 2")
def greet_f(file):
    """Описание команды 2"""
    click.echo(f"Hello {file}")


# Добавляем в группу команду
cli.add_command(greet_p)
# Добавляем в группу команду
cli.add_command(greet_f)

if __name__ == '__main__':
    cli()
```

```bash
╭─------------------------------------------------
╰─➤  python console.py --help
Usage: console.py [OPTIONS] COMMAND [ARGS]...

  Описание группы

Options:
  --help  Show this message and exit.

Commands:
  nameone  Краткое описание команды 1
  nmaetwo  Описание команды 2

╭─------------------------------------------------
╰─➤  python console.py nameone --help
Usage: console.py nameone [OPTIONS] MODE

  Полное описание команды 1

Options:
  -p, --path TEXT  Описание флага 1
  --help           Show this message and exit.

```

## Типы аргументов и параметров

[Типы](https://click.palletsprojects.com/en/8.0.x/parameters/):

- `str`
- `int`
- `float`
- `bool`
- `click.File`(`mode='r', encoding=None, errors='strict', lazy=None, atomic=False`) - Тип для работы с файлом, мы получаем сразу дескриптор [файла](https://click.palletsprojects.com/en/8.0.x/arguments/#file-arguments)

    - `mode:str='r'` - Режим работы `w/r/b/a`
    - `encoding:str` - Кодировка
    - `atomic:bool=False` - Сделать копию файла и отдать от него дескриптор, а в момент выхода из функции он переместиться в исходный путь.

- `click.Path`(`exists=False, file_okay=True, dir_okay=True, writable=False, readable=True, resolve_path=False, allow_dash=False, path_type=None`) - Тип для файлов

    - `exists:bool=Fale` — файл или каталог должны существовать, чтобы значение быть действительным. Если это не установлено True, а файл не существуют, то все дальнейшие проверки молча пропускаются.
    - `file_okay:bool=True` — Разрешить файл в качестве значения.
    - `dir_okay:bool=True` — Разрешить каталог в качестве значения.
    - `writable:bool=False` — файл или каталог должны быть доступны для записи.
    - `readable:bool=False` — файл или каталог должны быть доступны для чтения.
    - `resolve_path:bool=False` — сделать значение абсолютным и разрешить любой символические ссылки. А ~не расширяется, как это должно быть выполняется только оболочкой.
    - `allow_dash` — Разрешить одиночный тире в качестве значения, которое указывает стандартный поток (но не открывает его). Использовать open_file()для обработки открытия этого значения.
    - `path_type:Any=None` - преобразовать значение входящего пути в указанный тип. Если `None`, сохраните значение Python по умолчанию, которое `str`. Полезно для преобразовать в `pathlib.Path`.

- `click.Choice`(`choices, case_sensitive=True`) - Ограничить варианты значений

    - `choices:list[str]` - список доступных слов
    - `case_sensitive:bool=True` - Быть ли проверки чувствительным к регистру

- `click.IntRange`(`min=None, max=None, min_open=False, max_open=False, clamp=False`) - Радиус целых чисел

    - `min:int` - Нижняя граница
    - `max:int` - Верхняя граница

- `click.FloatRange`(`min=None, max=None, min_open=False, max_open=False, clamp=False`) - Радиус дробных чисел

    - `min:int` - Нижняя граница
    - `max:int` - Верхняя граница

- `click.DateTime`(`formats=None`) - Получить дату
    - `formats:str` - формат даты `'%Y-%m-%d %H:%M:%S'`

```python
import click


@click.command()
@click.option('--hash-type',
 type=click.Choice(['MD5', 'SHA1'], case_sensitive=False))
def digest(hash_type):
    click.echo(hash_type)


if __name__ == '__main__':
    digest()


#╰─➤  python console.py --hash-type md5                                                                    2 ↵
#MD5
```

# Тестирование `Click`

[Про тестирование](https://click.palletsprojects.com/en/8.0.x/testing/)

Код тестирования

```python
from click.testing import CliRunner

from console import cli


def test_cod():
	# Создаем обьект
    runner = CliRunner()
	# ВЫзываем команду
    result = runner.invoke(cli, ['nameone', '-p', 'sm.py', "helpful.py"])
	# Код ответа
    assert result.exit_code == 0
	# Текст ответа
    assert result.output == 'Hello Peter!\n'


if __name__ == '__main__':
    test_hello_world()
```

Что тестируем

```python
import click


# Создаем группу
@click.group()
def cli():
    """Описание группы"""
    pass


@click.command("nameone", short_help="Краткое описание команды 1")  # Указываем внешнее имя для команды
@click.option('path', "--path", '-p', type=str, help="Описание флага 1")
@click.argument('mode')
def greet_p(path, mode):
    """Полное описание команды 1"""
    click.echo(f"Hello {path}, {mode}")


@click.command("nmaetwo")  # Указываем внешнее имя для команды
@click.option('file', "--file", '-f', type=str, help="Описание флага 2")
def greet_f(file):
    """Описание команды 2"""
    click.echo(f"Hello {file}")


# Добавляем в группу команду
cli.add_command(greet_p)
# Добавляем в группу команду
cli.add_command(greet_f)

if __name__ == '__main__':
    cli()

```
