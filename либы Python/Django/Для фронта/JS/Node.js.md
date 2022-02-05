---
date created: 2021-12-26 18:12
date updated: 2021-12-27 20:44
---

# Node.js

## Вступление

`Node.js` — программная платформа, основанная на движке V8 (транслирующем JavaScript в машинный код), превращающая JavaScript из узкоспециализированного языка в язык общего назначения. Node.js добавляет возможность JavaScript взаимодействовать с устройствами ввода-вывода через свой API, написанный на C++, подключать другие внешние библиотеки, написанные на разных языках, обеспечивая вызовы к ним из JavaScript-кода. Node.js применяется преимущественно на сервере, выполняя роль веб-сервера, но есть возможность разрабатывать на Node.js и десктопные оконные приложения (при помощи NW.js, AppJS или Electron для Linux, Windows и macOS) и даже программировать микроконтроллеры (например, tessel, low.js и espruino). В основе Node.js лежит событийно-ориентированное и асинхронное (или реактивное) программирование с неблокирующим вводом/выводом.

---

[Установка самой новой версии `node.js`](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-20-04-ru)

```bash
cd ~
curl -sL https://deb.nodesource.com/setup_14.x -o nodesource_setup.sh
sudo bash nodesource_setup.sh
sudo apt install nodejs
node -v # 14.x.x
```

Установить стабильную версию `node.js`

```bash
sudo apt install nodejs
```

| Команда `npm`                                        | Описание                                                                                                               |
| ---------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| `npm init`                                           | Инициализировать проект. Создастся файл `package.json` и папка `node_modules`                                          |
| ---                                                  | ---                                                                                                                    |
| `npm install <jquery>` / `npm install jquery --save` | Установить библиотеку в проект, после этого библиотека добавиться в зависимости проекта `package.json->"dependencies"` |
| `npm install <jquery> --save-dev`                    | Установить библиотеку, и занести её в список зависимостей для разработки(тестирования).                                |
| `npm install`                                        | Установить все зависимости из файла `package.json` (`devDependencies` и `dependencies` )                               |
| `npm install --dev`                                  | Установить все зависимости для разработки `devDependencies`                                                            |
| `npm install --production`                           | Установить все зависимости для диплоя `dependencies`                                                                   |
| ---                                                  | ---                                                                                                                    |
| `npm start`                                          | Запустить проект                                                                                                       |
| `npm run <Команда>`                                  | Выполнить команду из `package.json->"scripts"`                                                                         |
|                                                      |                                                                                                                        |

## Импорт

1. Проверить что мы запускаем сам файл, а не импортируем. (`if __name__ == "__main__":`)

    ```ts
    if (require.main === module) {
    	...;
    }
    ```

1. Экспорт сущности

    ```ts
    export const a = 10
    export function b(){}
    ```

1. Импорт сущности

    ```ts
    const {a, b} = require("$ПутьК_Файлу_Без$");
    ```

## Работа с файлом переменных окружений

Прочитать файл с переменными окружения

```js
// Считываем переменные окружения из файла `npm install dotenv`
const envy = require("dotenv").config({ path: "./ИмяФайла.env" });
//Получаем имя проекта из переменных окружения
const ProjName = envy.parsed.NAME_PROJ;
console.log(ProjName);
```

> Нужно иметь библиотеку `dotenv`
>
> ```bash
> npm install dotenv
> ```

## Работа с файлами

Импортируем модуль для работы с файлами

```ts
const fs = require('fs')
```

- Прочитать файл

    ```ts
    res = fs.readFileSync('$ПутьК_Файлу$', 'utf8')
    ```

- Записать данные в файл

    ```ts
    res = fs.writeFileSync(`$ПутьК_Файлу$`, "Данные");
    ```

- Дозаписать в файл

    ```ts
    res = fs.appendFileSync(`$ПутьК_Файлу$`, "Данные");
    ```

- Проверить существование папки или файла
    ```ts
    res = fs.existsSync(`$ПутьК_Файлу$`)
    ```

## Работа с аргументами командной строки

Скачать `yargs

```bash
npm install yargs --save-dev
```

Использование: `123123 --save sd --dev -d affsad`

```ts
const argsTerminal = require('yargs').argv;
console.log(argsTerminal);
/*
{
  "_": [
    123123
  ],
  "save": "sd",
  "dev": true,
  "d": "affsad",
  "$0": "Ne.ts"
}
*/
```

## Shell

Выполнить команду в консоль

```ts
exec("$Команды$", (error : ExecException | null, stdout : string, stderr : string) => {
			if (error) {
				console.log(`error: ${error.message}`);
				return;
			}
			if (stderr) {
				console.log(`stderr: ${stderr}`);
				return;
			}
			// Вывести результат команды
			console.log(`stdout: ${stdout}`);
		});
```
