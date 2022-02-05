# Установка

- [Быстрый экскурс](https://pythonru.com/biblioteki/vvedenie-v-sqlalchemy)
- [Официальная Документация](https://docs.sqlalchemy.org/en/14/tutorial/index.html)

Установить через `pip` (`psycopg2-binary` драйвер для работы с `PostgreSQL`)

```bash
poetry add SQLAlchemy psycopg2-binary
```

# Двигатели и пул соединений

**Пул соединений** — это стандартный способ кэширования соединений в памяти, что позволяет использовать их повторно.
Создавать соединение каждый раз при необходимости связаться с базой данных — затратно. А пул соединений обеспечивает
неплохой прирост производительности.

При таком подходе приложение при необходимости обратиться к базе данных вытягивает соединение из пула. После выполнения
запросов подключение освобождается и возвращается в пул. Новое создается только в том случае, если все остальные
связаны.

Для создания движка (объекта Engine) используется функция create_engine() из пакета sqlalchemy. В базовом виде она
принимает только строку подключения. Последняя включает информацию об источнике данных. Обычно это приблизительно
следующий формат:

```python
from sqlalchemy import create_engine

# Подключение к серверу PostgreSQL на localhost с помощью psycopg2 DBAPI engine = create_engine("postgresql+psycopg2://username:password@localhost/mydb")
# Подключение к серверу MySQL на localhost с помощью PyMySQL DBAPI. engine = create_engine("mysql+pymysql://username:password@localhost/mydb")
# Подключение к серверу MySQL по ip 23.92.23.113 с использованием mysql-python DBAPI. engine = create_engine("mysql+mysqldb://username:password@23.92.23.113/mydb")
# Подключение к серверу Oracle на локальном хосте с помощью cx-Oracle DBAPI.
engine = create_engine("oracle+cx_oracle://username:password@localhost/mydb")
# Подключение к MSSQL серверу на localhost с помощью PyODBC DBAPI.
engine = create_engine("oracle+pyodbc://username:password@localhost/mydb")
# Подключиться к SQLite используя относительный путь
engine = create_engine('sqlite:///sqlite3.db')
# Подключиться к SQLite используя абсолютный путь
engine = create_engine('sqlite:////path/to/sqlite3.db')
```

- `create_engine($url$, $...$)` - Создать пул соединений

    - `url` - Адрес подключения к СУБД `dialect+driver://username:password@host:port/database`
    - `dialect` — это имя базы данных (`mysql`, `postgresql`, `mssql`, `oracle` и так далее).

        - `driver` — используемый DBAPI. Этот параметр является необязательным. Если его не указать будет использоваться  
             драйвер по умолчанию (если он установлен).
        - `username` - имя
        - `password` — данные для получения доступа к базе данных.
        - `host` — расположение сервера базы данных.
        - `port` — порт для подключения.
        - `database` — название базы данных.

    - `echo` - Булево значение. Если задать `True`, то движок будет сохранять логи `SQL` в стандартный вывод. По  
         умолчанию значение равно `False`
    - `pool_size` - Определяет количество соединений для пула. По умолчанию — 5
    - `max_overflow` - Определяет количество соединений вне значения pool_size. По умолчанию — 10
    - `encoding` Определяет кодировку `SQLAlchemy`. По умолчанию — `UTF-8`. Однако этот параметр не влияет на  
         кодировку всей базы данных
    - `isolation_level` - Уровень изоляции. Эта настройка контролирует степень изоляции одной транзакции. Разные базы  
         данных поддерживают разные уровни. Для этого лучше ознакомиться с документацией конкретной базы данных

# Работа с существующими БД

Когда нам нужно взаимодействовать с БД которая создана вне моделей `SqlAlchemy`. Мы можем сто сделать обратившись к, мета данным БД

```python
from sqlalchemy import create_engine, MetaData
from sqlalchemy.orm import sessionmaker

engine = create_engine("postgresql+psycopg2://postgres:root@localhost/fast", echo=False)
Session = sessionmaker(bind=engine)
session = Session()

meta = MetaData()
# Получем таблицы
meta.reflect(bind=engine)

obj_ = Table("$ИмяТаблицы$", meta) # meta.tables["$ИмяТаблицы$"]


if __name__ == '__main__':
	#  Обращаемся к таблице в стиле ORM
    print(session.query(obj_.c.$Столбец$).all())
```

Для того чтобы обратиться к столбцу таблице, в данном случае нужно обратиться к атрибуту `$Таблица$.c.$ИмяСтолбца$`

---

Мы можем создать класс и связать его с таблицей в БД. Это более предпочтительный вариант.

```python
from sqlalchemy import create_engine, MetaData
from sqlalchemy.orm import sessionmaker

engine = create_engine("postgresql+psycopg2://postgres:root@localhost/fast", echo=False)
Session = sessionmaker(bind=engine)
session = Session()

meta = MetaData()
# Получем таблицы
meta.reflect(bind=engine)

obj_ = Table("$ИмяТаблицы$", meta) # meta.tables["$ИмяТаблицы$"]


class Model:
	# можно оставить пустым
    ...


# Связываем таблицу с классом
mapper(Model, obj_)

if __name__ == '__main__':
	#  Обращаемся к таблице в стиле ORM
    print(session.query(Model.$Столбец$).all())
```

# `SQLAlchemy Core`

## CRUD

### Создать

[Документация про `SQl Core`](https://pythonru.com/biblioteki/crud-sqlalchemy-core#%D0%9F%D0%BE%D0%BB%D1%83%D1%87%D0%B5%D0%BD%D0%B8%D0%B5-%D0%B7%D0%B0%D0%BF%D0%B8%D1%81%D0%B5%D0%B9)

Вставить одно значение

```python
from sqlalchemy import create_engine, insert

engine = create_engine("postgresql+psycopg2://postgres:root@localhost/fast", echo=False)
Session = sessionmaker(bind=engine)
session = Session()
...

sql_ = insert($Модель$).values($Атрибут_...$=$Значение_...$)

# Получить RAW SQL команду
print(sql_)
# Получить словарь с параметрами вставки
print(sql_.compile().params)
# Выполнить команду
_res = session.execute(sql_)
# Получить pk новой созданной строки
_res.inserted_primary_key
```

Вставить несколько значений

```python
_res = session.execute(insert($Модель$),[
 { "$Атрибут_...$": "$Значение_...$" }, { "$Атрибут_...$": "$Значение_...$" }, ])

# Сколько строк добавлено
print(_res.rowcount)
```

Вложенный запрос [+](https://docs.sqlalchemy.org/en/14/core/dml.html#sqlalchemy.sql.expression.Insert.from_select)

```python
insert($Модель_1$).from_select([$ИмяСтолбеца_...$,], select($Модель_2$))
```

### Прочитать

```python
from sqlalchemy import create_engine, insert

engine = create_engine("postgresql+psycopg2://postgres:root@localhost/fast", echo=False)
Session = sessionmaker(bind=engine)
session = Session()
...

# Создадим SQL команду
sql_ = select(User.published)
# Выполним запрос
_res = session.execute(sql_)
# Получим данные
print([x for x in _res])
# Или можно получить все записи так
print(_res.fetchall())
```

- `fetchone()` Получить следующую запись.
- `fetchall()` Получить все записи.
- `fetchmany(size=$Сколько$)` Получить указанное количество данных, если из нет вернуть `None`
- `first()` Получить первую запись и закрыть соединение
- `rowcount` Количество строк в результате
- `keys()` Возвращает список колонок из источника данных
- `scalar()` Возвращает первую колонку первой записи и закрывает соединение. Если результата нет, то возвращает None

---

Условие `WHERE`

```python
sql_ = select($Модель$).where($Модель$.$Атриубт$ == $Значение$)
session.execute(sql_).fetchall()
```

Объединение условий

```python
sql_ = select($Модель$).where(
 ($Модель$.$Атриубт$ == $Значение$) & ($Модель$.$Атриубт$ == $Значение$))
session.execute(sql_).fetchall()
```

- `&` - `AND`
- `|` - `OR`
- `~` - `NOT`

#### Объединить запросы

`union, union_all`

```python
from sqlalchemy import select, union, union_all

# Удалить повторяющийся результат
sql_ = union(
 select($Модель$).where(...), select($Модель$).where(...),)
session.execute(sql_)

# Без удаления повторяющегося результата
sql_ = union_all(
 select($Модель$).where(...), select($Модель$).where(...),)
session.execute(sql_)
```

#### Подзапросы

`select_from`

```python
sql_ = union(
 select($Модель_1$).select_from($Модель_2$),)
session.execute(sql_)
```

### Обновить

```python
from sqlalchemy import update

sql_ = update($Модель$).where(...).values($Атриут$ = $НовоеЗначение$)
session.execute(sql_)
```

### Удалить

```python
from sqlalchemy import delete

sql_ = delete($Модель$).where(...).values($Атриут$ = $НовоеЗначение$)
session.execute(sql_)
```

## `RAW` Запросы

[Про RAW запросы](https://chartio.com/resources/tutorials/how-to-execute-raw-sql-in-sqlalchemy/)

```python
from sqlalchemy import create_engine, text
from sqlalchemy.orm import sessionmaker

engine = create_engine("postgresql+psycopg2://postgres:root@localhost/fast", echo=False)
Session = sessionmaker(bind=engine)
session = Session()
...

sql_ = text("""SELECT * FROM users""")

print([x for x session.execute(sql_)])
```

Можно использовать шаблоны. Для этого нужно в сырой `SQL`команде указать `:Ключ`, а в сессии передать словарь `{"Ключ":"Значение"}`

```python
sql_ = text("""SELECT * FROM users WHERE id > :max""")
_res = session.execute(sql_,{"max":'2'})
print(_res.fetchall())
```

# `ORM`

## Введение

[+](https://pythonru.com/biblioteki/shemy-v-sqlalchemy-orm)

**Модель** — это класс Python, соответствующий таблице в базе данных, а его свойства — это колонки.

Чтобы класс был валидной моделью, нужно соответствовать следующим требованиям:

1. Наследоваться от декларативного базового класса с помощью вызова функции `declarative_base()`.
2. Объявить имя таблицы с помощью атрибута `__tablename__`.
3. Объявить как минимум одну колонку, которая должна быть частью первичного ключа

**Декларативный базовый класс** — это оболочка над маппером и `MetaData`. Маппер соотносит подкласс с таблицей,  
а `MetaData` сохраняет всю информацию о базе данных и ее таблицах. (`declarative_base()`)

**Сессии** - При использовании `SQLAlchemy ORM` взаимодействие с базой данных происходит через объект `Session`. Он  
также захватывает соединение с базой данных и транзакции. Транзакция неявно стартует как только `Session` начинает  
общаться с базой данных и остается открытой до тех пор, пока `Session` не коммитится, откатывается или закрывается.  
(`SESSION = sessionmaker(bind=engine)`)

---

## Создание модели

При использовании ORM ключи и ограничения добавляются с помощью атрибута `__table_args__`.

- `__tablename__` - Имя таблицы
- `__table_args__` - Ограничители таблицы `( PrimaryKeyConstraint('id', name='user_pk'), UniqueConstraint('username'), UniqueConstraint('email'), )`

```python
# Подключаемся к СУБД
engine = create_engine("postgresql+psycopg2://postgres:root@localhost/fast")
# Декларативный базовый класс
Base = declarative_base()
```

- Создать все модели из БД

    ```python
    Base.metadata.create_all(engine)
    ```

- Удалить все модели из БД

    ```python
    Base.metadata.drop_all(engine)
    ```

## CRUD

### Создать

Для добавления данных в БД мы можем использовать конструктор модели.

```python
# engine = create_engine("postgresql+psycopg2://postgres:root@localhost/fast", echo=True)
# Session = sessionmaker(bind=engine)
# session = Session()
# Base = declarative_base()
# class $Models$(Base):
#       $Аргумент_...$=$Значение_...$,

obj_ = $Models$($Аргумент_...$=$Значение_...$,)
```

Для того чтобы записать данные в БД сначала нужно добавить экземпляр класса в сессию

```python-tl
session.add($obj_$)
```

> Добавить несколько экземпляров
>
> ```python-tl
> session.add_all([$obj_...$, ])
> ```
>
> Посмотреть список задач в сессии  
> `session.new`

После этого нужно зафиксировать изменения.

```python
session.commit()
```

---

Если в СУБД возникло исключение, то нужно создать новую сессию

```python
session = Session()
```

### Прочитать

Чтобы сделать запрос в базу данных используется метод `session.query()` объекта `session`. Он возвращает объект  
типа `sqlalchemy.orm.query.Query`, который называется просто `Query`. Он представляет собой инструкцию `SELECT`, которая  
будет использована для запроса в базу данных. В следующей таблице перечислены распространенные методы класса Query.  
Метод Описание

- `session.query($Model$.$Атриут_...$,)` - Указывает из какой таблицы и из каких столбцов мы будем брать данные.
- `.all()` - Возвращает результат запроса (объект Query) в виде списка
- `.count()` - Возвращает общее количество записей в запросе
- `.first()` - Возвращает первый результат из запроса или None, если записей нет
- `.scalar()` Возвращает первую колонку первой записи или `None`, если результат пустой. Если записей несколько, то бросает исключение `MultipleResultsFound`
- `.one()` Возвращает одну запись. Если их несколько, бросает исключение `MutlipleResultsFound`. Если данных нет, бросает `NoResultFound`
- `.get(pk)` - Возвращает объект по первичному ключу (pk) или None, если объект не был найден
- `.filter($Model$.$Атриубт$ == $Значение$)` - Возвращает экземпляр Query после применения оператора `WHERE`

    - Для объединения условия можно использовать. Или `from sqlalchemy import select, and_, or_, not_`

        - `&` - `and_()` `.filter(($Model$.$Атриубт$ == $Значение$) & ($Model$.$Атриубт$ == $Значение$))`
        - `|` - `or_()` `.filter(($Model$.$Атриубт$ == $Значение$) | ($Model$.$Атриубт$ == $Значение$))`
        - `~` - `not_()` `.filter(~($Model$.$Атриубт$ == $Значение$))`

    - `LIKE ` = `$Model$.$Атриубт$.like($Шаблон$)` поиск текст по шаблону с учетом регистр
    - `ILIKE` = `$Model$.$Атриубт$.ilike($Шаблон$)` поиск текст по шаблону с БЕЗ учета регистра
    - `IS NULL` = `$Model$.$Атриубт$ == None`
    - `IS NOT NULL` = `$Model$.$Атриубт$ != None`
    - `IN` = `$Model$.$Атриубт$.in_([$Значение_...$])` Значение есть в указанном массиве
    - `NOT IN` = `$Model$.$Атриубт$.notin_([$Значение_...$])`
    - `BETWEEN` = `$Model$.$Атриубт$.between_($Старт$,$Стоп$)` Между
    - `NOT BETWEEN` = `not_($Model$.$Атриубт$.between_($Старт$,$Стоп$))`
    - [Все операторы](https://docs.sqlalchemy.org/en/14/core/operators.html)

- `.limit($Макс$)` Возвращает экземпляр `Query` после применения оператора `LIMIT` ограничить количество результата
- `.offset($СколькоПропустить$)` Возвращает экземпляр `Query` после применения оператора `OFFSET` пропустить с начало
- `.order_by($Model$.$Атриубт$)` Возвращает экземпляр `Query` после применения оператора `ORDER BY`

    - Отсортировать в обратном порядке `from sqlalchemy import desc` `.order_by(desc($Model$.$Атриубт$))`

- `($Model_1$,$Model_2$).join($Model_2$)` Возвращает экземпляр `Query` после создания `SQL` INNER `INNER JOIN`
- `.outerjoin($Model$)` Возвращает экземпляр `Query` после создания `SQL` `LEFT OUTER JOIN`

    - создать `FULL OUTER JOIN` можно, передав в метод `full=True`

- `.group_by($Model$.$Атриубт$)`- Возвращает экземпляр `Query` после добавления оператора `GROUP BY` к запросу

    - `session.query(func.$Фнкция$($Model$.$Атриубт$)).group_by($Model$.$Атриубт$).all()` [Агрегирующие функции](#Агрегирующие%функции)

- `.having(func.$Функция$ == Значение$)` Возвращает экземпляр `Query` после добавления оператора `HAVING`
- `.distinct()` - Только уникальные значения `DISTINCT`
- `cast($Занчение$, $SQL_ТИП$)` - Приведение типов

    ````python
    from sqlalchemy import cast session.query(cast($Занчение$, $SQL_ТИП$))
    ```- `$Запрос_1$.union($Зарос_2$)` - Объединить `SQL` запросы.
    - По умолчанию он удаляет повторения, для того чтобы получить повторения
       используете `$Запрос_1$.union_all($Зарос_2$)`

    ````

> Чтобы получить сырой `SQL` мы можем вызвать функцию `print(session.query($...$))`

#### Агрегирующие функции

[Документация](https://docs.sqlalchemy.org/en/14/core/functions.html)

```python
from sqlalchemy import func, create_engine, sessionmaker

engine = create_engine("postgresql+psycopg2://postgres:root@localhost/fast", echo=False)
Session = sessionmaker(bind=engine)
session = Session()

Base = declarative_base()

session.query(func.$Фнкция$($Model$.$Атрибут$))
```

- `func.max()`
- `func.min()`
- `func.count()`
- `func.sum()`
- `func.lower("")` - В нижний регистр
- `func.upper("")` - В верхний регистр
- `func.length("")` - Длинна строки
- `func.trim("")` - Убрать пробелы по бокам
- `func.floor()` - Округлить
- `func.ceil()` - Округлить

---

- `AnsiFunction`
- `array_agg`
- `char_length`
- `coalesce`
- `concat`
- `cube`
- `cume_dist`
- `current_date`
- `current_time`
- `current_timestamp`
- `current_user`
- `dense_rank`
- `Function`
- `FunctionAsBinary`
- `FunctionElement`
- `GenericFunction`
- `grouping_sets`
- `localtime`
- `localtimestamp`
- `mode`
- `next_value`
- `now`
- `OrderedSetAgg`
- `percent_rank`
- `percentile_cont`
- `percentile_disc`
- `random`
- `rank`
- `ReturnTypeFromArgs`
- `rollup`
- `ScalarFunctionColumn`
- `session_user`
- `sysdate`
- `user`

### Обновить

Обновить данные в одной записи

```python
obj_ = session.query($Модель$).get($pk$)
obj_.$Атрибут$ = $НовоеЗначение$
session.add(obj_)
session.commit()
```

Обновить данные через условие

```python
session.query($Модель$).filter(...).update({"$Атрибут_...$": $НовоеЗначение$}, synchronize_session = 'fetch')
session.commit()
```

[Про `synchronize_session`](https://docs.sqlalchemy.org/en/14/orm/session_basics.html#selecting-a-synchronization-strategy)

### Удалить

Обновить данные в одной записи

```python
obj_ = session.query($Модель$).get($pk$)
obj_.$Атрибут$ = $НовоеЗначение$
session.delete(obj_)
session.commit()
```

Обновить данные через условие

```python
session.query($Модель$).filter(...).delete(synchronize_session='fetch')
session.commit()
```

## Транзакции

[Про транзакции](https://docs.sqlalchemy.org/en/14/orm/session_transaction.html)

Самый надежный вариант транзакции. Так как если в сиссе возникнут ошибки данные не повредятся и отчистятся

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker
engine = create_engine("postgresql+psycopg2://postgres:root@localhost/fast", echo=False)
Session = sesssionmaker(engine)

...

with Session.begin() as session:
    session.add(some_object):
```

---

Более оптимизированный вид транзакции, в этом случае мы уже не контролируем сессии.
В конце приведенного выше контекста, при условии, что исключений не возникало, любые ожидающие объекты будут сброшены в базу данных, а транзакция базы данных будет совершено. Если в указанном выше блоке возникло исключение, то транзакция будет отменена. В обоих случаях вышеизложенное `Session` после выхода из блока готов к использованию в последующие сделки.

```python
session = Session()
...

with session.begin():
    session.add($Команда$)
```

Без контекстного менеджера

```python
session = Session()
...

try:
	session.add($Команда$)
	session.commit()
	print("Транзакция завершена.")
except IntegrityError as e:
	print(e)
	print("Возврат назад...")
	session.rollback() # Откатить изменения
	print("Транзакция не удалась.")
```

# Связи между моделями

`relationship` Позволяет более удобно создавать и читать данные из нескольких таблиц

Особенности `relationship` :

- Эти связи виртуальные, они не существуют в БД. Сам `SqlAlchemy` реализует логику работы этих связей (делает доп
    запросы, и объединяет результат)
- В любой момент можно создать или удалить `relationship` так как это виртуальная связь
- Если нам нужно связать модели в обе стороны, то можно использовать такой вариант записи. В одной из модели мы
    указываем `$НовыйАтрибутТекщейМодели$ = relationship("$ДругаяМодель$", backref="$ИмяАтрибутаВ_ДругойМодели$")`, после
    этого у нас будет связь в две стоны

---

`$Атриубт$ = relationship("$ИмяМодели$")` - [+](https://docs.sqlalchemy.org/en/14/orm/relationship_api.html#sqlalchemy.orm.relationship)

- `backref="ИмяВ_ДругойТаблице"` - Создайте обратную ссылку на модель, с указанным именем

- `lazy='select` - указывает Как должны быть загружены связанные элементы.

    **Ленивые**

    - `select` - элементы должны загружаться **лениво**, когда свойство первый доступ с использованием отдельного оператора SELECT или карты идентификаторов fetch для простых ссылок «многие к одному». (Если обратиться к атрибуту то происходит доп запрос)

    - `dynamic` - атрибут вернет предварительно настроенный `Query` объект для всех прочитанных операций, на которые могут быть наложены дальнейшие операции фильтрации. применяется до повторения результатов. (При обращение к атрибуту возвращает `sql` запрос, для получения данных об этом атрибуте ) `session.execute(sql_[0].$Атрибут$).fetchall()`

    **Жадные**

    - `joined` - элементы должны загружаться **жадно** в том же запросе, что и родителя, используя `JOIN` или `LEFT OUTER JOIN`. Ли соединение является «внешним» или нет, определяется `relationship.innerjoin` параметр. (Всегда делает запрос джоном)

    - `immediate` - элементы должны загружаться по мере загрузки родителей, с помощью отдельного оператора `SELECT` или выборки идентификационной карты для простые ссылки «многие к одному». (Делает сразу несколько под запросов)

    - `subquery` - предметы должны быть загружены «жадно», как и родители загружается с использованием одного дополнительного оператора SQL, который выполняет JOIN для подзапрос исходного оператора для каждой коллекции просил. (Сначала селект потом под запрос потом джоин)

    - `selectin` - элементы должны загружаться «жадно», как и родители загружаются с помощью одного или нескольких дополнительных операторов SQL, которые выдает JOIN непосредственному родительскому объекту, указывая первичный идентификаторы ключей с использованием предложения IN. (Сначала селект потом селект с условием `IN`)

    **Не загружать**

    - `noload` - никакая загрузка не должна происходить в любое время. Это для поддерживать атрибуты «только для записи» или атрибуты, которые заполняется некоторым образом, характерным для приложения. (Не загружает, в ответе пустые данные)

    - `raise` - ленивая загрузка запрещена; доступ атрибут, если его значение еще не было загружено через нетерпеливое загрузка, поднимет `InvalidRequestError`. Эту стратегию можно использовать, когда объекты должны быть отсоединены от их прикрепленный `Session` после их загрузки. (Ошибка при попытки доступа)

    - `raise_on_sql` - запрещена ленивая загрузка, выдающая SQL; доступ к атрибуту, если его значение еще не было загружено через нетерпеливая загрузка, поднимет `InvalidRequestError`, если ленивая загрузка должен выдать SQL . Если ленивая загрузка может вытащить соответствующее значение из карты удостоверений или определить, что это должно быть None, значение загружено. Эту стратегию можно использовать, когда объекты оставаться связанным с прикрепленным Session, тем не мение дополнительные операторы SELECT должны быть заблокированы.

    - `True` - `select`

    - `False` - `joined`

    - `None` - `noload`

### Один ко многим

```python
from pprint import pprint

from sqlalchemy import Integer, String, Column, ForeignKey
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker, relationship

engine = create_engine("postgresql+psycopg2://postgres:root@localhost/fast", echo=True)
Session = sessionmaker(bind=engine)
session = Session()
Base = declarative_base()


class Profile(Base):
    __tablename__ = 'пользователь_'
    id = Column(Integer, primary_key=True)
    f_name = Column(String(255), nullable=False)
    l_name = Column(String(255), nullable=False)
    photo = relationship("Photo", backref="profile")

    # photo = relationship("Photo")
    def __repr__(self):
        return f"{self.id=},{self.f_name=}"


class Photo(Base):
    __tablename__ = 'фотографии_'
    id = Column(Integer, primary_key=True)
    id_user = Column(Integer, ForeignKey("пользователь_.id"))
    path_image = Column(String(600), nullable=False)

    # profile = relationship("Profile")
    def __repr__(self):
        return f"{self.id=},{self.id_user=},{self.path_image=}"


def insert_():
    ob = Profile(f_name="Петя", l_name="Федоров")
    # Так как у нас есть связи мы можем обратиться к атрибуту `photo`
    ob.photo = [
        Photo(path_image="Фото_1"),
        Photo(path_image="Фото_2"),
        Photo(path_image="Фото_3"),
    ]
    session.add(ob)
    session.commit()

    """
    | id  | f_name | l_name  |
    |:----|:-------|:--------|
    | 1   | Петя   | Федоров |

    | id  | id_user | path_image |
    |:----|:--------|:-----------|
    | 1   | 1       | Фото_1     |
    | 2   | 1       | Фото_2     |
    | 3   | 1       | Фото_3     |
    """


def read_Profile():
    sql_ = session.query(Profile).all()
    # SELECT * FROM "пользователь_"
    pprint(sql_)
    """
    [self.id=1,self.f_name='Петя']
    """
    # Если мы обращаемся к атрибут `photo` то будет произведен ДОП запрос
    # SELECT * FROM "фотографии_" WHERE %(param_1)s = "фотографии_".id_user"
    pprint(sql_[0].photo)
    """
    [
        self.id=1,self.id_user=1,self.path_image='Фото_1',
        self.id=2,self.id_user=1,self.path_image='Фото_2',
        self.id=3,self.id_user=1,self.path_image='Фото_3'
    ]
    """


def read_Photo():
    sql_ = session.query(Photo).all()
    # SELECT * FROM "фотографии_"
    pprint(sql_)
    """
    [
        self.id=1,self.id_user=1,self.path_image='Фото_1',
        self.id=2,self.id_user=1,self.path_image='Фото_2',
        self.id=3,self.id_user=1,self.path_image='Фото_3'
    ]
    """
    # Если мы обращаемся к атрибут `photo` то будет произведен ДОП запрос
    # SELECT * FROM "пользователь_" WHERE "пользователь_".id = %(pk_1)s"
    pprint(sql_[0].profile)
    """
    self.id=1,self.f_name='Петя'
    """


if __name__ == '__main__':
    # Base.metadata.create_all(engine)
    # insert_()
    read_Profile()
    read_Photo()
```

### Один к одному

```python
from pprint import pprint

from sqlalchemy import Integer, String, Column, ForeignKey
from sqlalchemy import create_engine
from sqlalchemy.exc import IntegrityError
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker, relationship

engine = create_engine("postgresql+psycopg2://postgres:root@localhost/fast", echo=True)
Session = sessionmaker(bind=engine)
session = Session()
Base = declarative_base()


class Profile(Base):
    __tablename__ = 'сотрудник_'
    id = Column(Integer, primary_key=True)
    f_name = Column(String(255), nullable=False)
    l_name = Column(String(255), nullable=False)
    position = Column(Integer(), nullable=False)

    pasport = relationship("Pasport", backref="profile", uselist=False)

    def __repr__(self):
        return f"{self.id=},{self.f_name=}"


class Pasport(Base):
    __tablename__ = 'паспорт_'
    id = Column(Integer, primary_key=True)
    # Так как связь Один к одному, то мы можем иметь
    # только уникальную(одну) ссылку на внешнюю таблицу
    id_person = Column(Integer, ForeignKey("сотрудник_.id"), unique=True)
    passport_number = Column(String(600), nullable=False)

    def __repr__(self):
        return f"{self.id=},{self.id_person=},{self.passport_number=}"


def insert_():
    ob = Profile(f_name="Петя", l_name="Федоров", position=5)
    # Так как у нас есть связи мы можем обратиться к атрибуту `photo`
    ob.pasport = Pasport(passport_number="Паспорт_1")
    session.add(ob)
    session.commit()

    """
    | id   | f_name | l_name  | position |
    | :--- | :---   | :---    | :---     |
    | 1    | Петя   | Федоров | 5        |

    | id   | id_person | passport_number |
    | :--- | :---      | :---            |
    | 1    | 1         | Паспорт_1       |
    """


def read_Profile():
    sql_ = session.query(Profile).all()
    # SELECT * FROM "сотрудник_"
    pprint(sql_)
    """
    [self.id=1,self.f_name='Петя']
    """
    # Если мы обращаемся к атрибут `pasport` то будет произведен ДОП запрос
    # SELECT * FROM "паспорт_" WHERE %(param_1)s = "паспорт_".id_person
    pprint(sql_[0].pasport)
    """
    [
        self.id=1,self.id_user=1,self.path_image='Фото_1',
        self.id=2,self.id_user=1,self.path_image='Фото_2',
        self.id=3,self.id_user=1,self.path_image='Фото_3'
    ]
    """


def read_Pasport():
    sql_ = session.query(Pasport).all()
    # SELECT * FROM "фотографии_"
    pprint(sql_)
    """
    [
        self.id=1,self.id_user=1,self.path_image='Фото_1',
        self.id=2,self.id_user=1,self.path_image='Фото_2',
        self.id=3,self.id_user=1,self.path_image='Фото_3'
    ]
    """
    # Если мы обращаемся к атрибут `photo` то будет произведен ДОП запрос
    # SELECT * FROM "сотрудник_" WHERE "сотрудник_".id = %(pk_1)s
    pprint(sql_[0].profile)
    """
    self.id=1,self.f_name='Петя'
    """


def two_insert():
    """
    При попытке записать второй паспорт на сотрудника
    должна произойти ошибка уникальности
    """
    res = False
    try:
        sql_ = Pasport(id_person=1, passport_number="Паспорт_2")
        session.add(sql_)
        session.commit()
    except IntegrityError:
        res = True

    assert res == True


if __name__ == '__main__':
    # Base.metadata.create_all(engine)
    # insert_()
    # two_insert()
    read_Profile()
    read_Pasport()
```

## Многие ко многим

```python
import logging
from pprint import pprint

from sqlalchemy import Integer, String, Column, ForeignKey
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker, relationship

##
yellow = "\t\x1b[93m"
reset = "\x1b[0m"
logging.basicConfig(format=f"%(levelname)s %(name)s {yellow}%(message)s{reset}", level="INFO")
logging.getLogger('sqlalchemy.engine.Engine').setLevel("INFO")
##

engine = create_engine("postgresql+psycopg2://postgres:root@localhost/fast")
Session = sessionmaker(bind=engine)
session = Session()
Base = declarative_base()


class Buyer(Base):
    __tablename__ = 'покупатель_'
    id = Column(Integer, primary_key=True)
    name = Column(String(255), nullable=False)
    email = Column(String(255), nullable=False, unique=True)
    phone = Column(String(255), nullable=False)

    basket = relationship("Basket", backref='buyer')

    def __repr__(self):
        return f"П {self.id=},{self.name=}"


class Product(Base):
    __tablename__ = 'продукт_'
    id = Column(Integer, primary_key=True)
    name = Column(String(255), nullable=False, unique=True)
    price = Column(Integer(), nullable=False)

    basket = relationship("Basket", backref='product')

    def __repr__(self):
        return f"Т {self.id=},{self.name=},{self.price=}"


class Basket(Base):
    __tablename__ = 'корзина_'
    id = Column(Integer, primary_key=True)
    id_buyer = Column(Integer, ForeignKey("покупатель_.id"), nullable=False)
    id_product = Column(Integer, ForeignKey("продукт_.id"), nullable=False)

    def __repr__(self):
        return f"К {self.id=},{self.id_buyer=},{self.id_product=}"


def insert_():
    """
    Так как пользователь и продукт, один и тот же,
    то мы его создаем единожды, а потом уже передает созданной экземпляр.
    """
    # Создаем пользователя
    sql_b = Buyer(name="Петя", email="peta@gmail.com", phone="+79989123123")
    # Создаем товар
    sql_p = Product(name="Колбаса", price=300)
    sql_ = [
        Basket(
            buyer=sql_b,
            product=Product(name="Молоко", price=50),
        ),
        Basket(
            buyer=sql_b,
            product=sql_p,
        ),
        Basket(
            buyer=Buyer(name="Катя", email="ката@gmail.com", phone="+41232341241"),
            product=sql_p,
        ),
    ]
    session.add_all(sql_)
    session.commit()


def read_Buyer():
    sql_ = session.query(Buyer).all()
    # SELECT * FROM "покупатель_"
    pprint(sql_)
    pprint(sql_[0].basket)


def read_Product():
    sql_ = session.query(Product).all()
    # SELECT * FROM "покупатель_"
    pprint(sql_)


def read_Basket():
    sql_ = session.query(Basket).all()
    # SELECT * FROM "покупатель_"
    pprint(sql_)


if __name__ == '__main__':
    # Base.metadata.create_all(engine)
    # insert_()
    read_Buyer()
    read_Product()
    read_Basket()
```

## Каскадное удаление обновление (111)

# Миграции (111)

# Асинхронность

# Логирование

## Красивый вывод

Переопределяем логгер `sqlalchemy` для красивого вывода исполняемых `SQL` команд. [Другие цвета](https://gist.github.com/iansan5653/c4a0b9f5c30d74258c5f132084b78db9)

```python
yellow = "\t\x1b[93m"
reset = "\x1b[0m"
logging.basicConfig(format=f"%(levelname)s %(name)s {yellow}%(message)s{reset}", level="INFO")
logging.getLogger('sqlalchemy.engine.Engine').setLevel("INFO")
```
