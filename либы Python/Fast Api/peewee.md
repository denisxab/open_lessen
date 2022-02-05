## Модели

### Создать модель

| Команда | Описание |
| ------- | -------- |
|         |          |
|         |          |

- `db.create_tables([$Модель_...$])` Создать модели в БД.
    - `safe:bool=True` (`IF NOT EXSIT`) создаст таблицу только если она не существует, если вам нужно пересоздать таблицу укажите `False`

```python
from peewee import PostgresqlDatabase, Model, CharField, DateField, IntegerField

db = PostgresqlDatabase('fast',
                        user='postgres',
                        password='root',
                        host='localhost',
                        port=5432)


class User(Model):
    name = IntegerField()
    birthday = DateField()

    class Meta:
        database = db


def main():
    with db:
        db.create_tables([User])


if __name__ == '__main__':
    main()
```

### Типы полей

https://docs.peewee-orm.com/en/latest/peewee/models.html#field-types-table

### `Meta`

https://docs.peewee-orm.com/en/latest/peewee/models.html?highlight=table%20generation#model-options-and-table-metadata
