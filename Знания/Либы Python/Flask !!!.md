## Команды

Запустить проект в режиме отладки

```python
os.environ["FLASK_ENV"] = "development"
```

## URL
### Относительные сслыки

## Шаблоны
```python
@app.route('/index')
def index():
    user = {'nickname': 'DEnis'}  # выдуманный пользователь
    return render_template("index.html",
                           title='Home',
                           user=user)
```


## Классы представления

https://flask.palletsprojects.com/en/2.0.x/views/