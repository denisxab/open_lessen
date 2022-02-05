# `uvicorn`

[Документация](https://www.uvicorn.org/)

| Команда                     | Описание                              |
| --------------------------- | ------------------------------------- |
| `uvicorn main:app --reload` | Запустить сервер с авто перезагрузкой |

---

Также можно запустить сервер из `Python`

```python
import uvicorn
from fastapi import FastAPI

app = FastAPI(version="1.2", default_response_class=ORJSONResponse)

if __name__ == "__main__":
    uvicorn.run(app, host="0.0.0.0", port=8000)
```
