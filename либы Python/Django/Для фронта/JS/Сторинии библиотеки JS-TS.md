## Loadash - Сборник готовых алгоритмов

Установить [+](https://www.cdnpkg.com/lodash.js/file/lodash.min.js/?id=51175)

```bash
npm i --save lodash @types/lodash 
```

Подключить

```ts
import * as _ from 'lodash';
```

---

`_.zipWith([arrays], [iteratee=_.identity])` =  Объединение массивов

```ts
_.zipWith([1, 2], [10, 20], [100, 200], function(a, b, c) {
  return a + b + c;
});
// => [111, 222]
```

## Ajv JSON  - Валидатор схемы

Установка [+](https://ajv.js.org/guide/getting-started.html)

```bash
npm install ajv
```
