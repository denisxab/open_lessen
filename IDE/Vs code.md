---
date created: 2021-12-28 01:17
date updated: 2021-12-28 13:43
---

## Настройки для `Vs code`

- Настройке `settings.json`

    ```json
    {
    	"workbench.colorTheme": "Default Dark+",
    	"redhat.telemetry.enabled": false,
    	/* Форматирование текста */
    	// Использвоать форматирование от палгина `Prettier`
    	"editor.defaultFormatter": "esbenp.prettier-vscode",
    	//  Форматировать документ при сохранение
    	"editor.formatOnSave": true,
    	// Путь к общему файлу с конфигурацйи форматирования палгина `Prettier`
    	// "prettier.prettierPath": "~/.prettierrc",
    	// "prettier.configPath": "~/.prettierrc",
    	/*                        */
    	"security.workspace.trust.untrustedFiles": "open",
    	"[javascript]": {
    		"editor.defaultFormatter": "esbenp.prettier-vscode"
    	},
    	"emmet.includeLanguages": {
    		"javascript": "javascriptreact"
    	},
    	"emmet.triggerExpansionOnTab": true,
    	"autoimport.doubleQuotes": true,
    	"autoimport.showNotifications": true,
    	"autoimport.filesToScan": "**/*.{ts,tsx,js,jsx}",
    	"cSpell.language": "en,ru",
    	"terminal.integrated.sendKeybindingsToShell": true,
    	"[typescript]": {
    		"editor.defaultFormatter": "esbenp.prettier-vscode"
    	},
    	// Изменять разрешение колесом мыши
    	"editor.mouseWheelZoom": true,
    	"files.exclude": {
    		"**/*.js": { "when": "$(basename).ts" },
    		"**/**.js": { "when": "$(basename).tsx" }
    	},
    	"grunt.autoDetect": "on",
    	"[yaml]": {
    		"editor.defaultFormatter": "esbenp.prettier-vscode"
    	},
    	"workbench.iconTheme": "vscode-icons",
    	"[json]": {
    		"editor.defaultFormatter": "esbenp.prettier-vscode"
    	},
    	"[jsonc]": {
    		"editor.defaultFormatter": "esbenp.prettier-vscode"
    	},
    	"prettier.jsxSingleQuote": true,
    	"prettier.jsxBracketSameLine": true,
    	"editor.formatOnPaste": true
    }
    ```

- Все плагинов

    ```bash
    code --install-extension aaron-bond.better-comments
    code --install-extension abusaidm.html-snippets
    code --install-extension batisteo.vscode-django
    code --install-extension CoenraadS.bracket-pair-colorizer-2
    code --install-extension dsznajder.es7-react-js-snippets
    code --install-extension eamodio.gitlens
    code --install-extension ecmel.vscode-html-css
    code --install-extension esbenp.prettier-vscode
    code --install-extension k--kato.intellij-idea-keybindings
    code --install-extension ms-azuretools.vscode-docker
    code --install-extension MS-CEINTL.vscode-language-pack-ru
    code --install-extension oderwat.indent-rainbow
    code --install-extension redhat.vscode-yaml
    code --install-extension steoates.autoimport
    code --install-extension streetsidesoftware.code-spell-checker
    code --install-extension streetsidesoftware.code-spell-checker-russian
    code --install-extension streetsidesoftware.code-spell-checker-spanish
    code --install-extension Tomi.xasnippets
    code --install-extension vscode-icons-team.vscode-icons
    code --install-extension xabikos.JavaScriptSnippets
    ```

    > Получить список плагинов. (В результате получаем текст для установки этих плагинов)
    >
    > ```bash
    > code --list-extensions | xargs -L 1 echo code --install-extension
    > ```

- `.prettierrc` - Для форматирования кода (плагин `Prettier`) [Prettier Красивое форматирование кода](Vs%20code.md#Prettier%20Красивое%20форматирование%20кода) [Документация по настройкам](https://github.com/prettier/prettier-vscode#prettierconfigpath) Глобальные настройки нужно хранить по пути `~/.prettierrc`

    ```json
    {
    	"tabWidth": 4,
    	"useTabs": true,
    	"arrowParens": "always",
    	"bracketSpacing": false,
    	"endOfLine": "lf",
    	"htmlWhitespaceSensitivity": "css",

    	"printWidth": 80,
    	"proseWrap": "preserve",
    	"quoteProps": "consistent",

    	"semi": true,
    	"jsxSingleQuote": true,
    	"singleQuote": true,
    	"trailingComma": "all"
    }
    ```

- `jsconfig.json` - Для умных импортов в `js`

    ```json
    {
    	"compilerOptions": {
    		"baseUrl": "./src",
    		"checkJs": true,
    		"jsx": "react"
    	}
    }
    ```

## Синхронизация настроек

- Создать конфигурацию:

    1.  Скачать плагин `Settings Sync`
    2.  Создать токен на `Githab`-> дать доступ к `gist`
        ![`Githab` токен](_attachments/205fc76a59e903fa31a87e53c64adb48.png)
    3.  Нажать `ctrl+shift+p` ввести `Sync: Update/Upload Settings`

- Скачать конфигурацию:

    1.  Нажать `ctrl+shift+p` ввести `Sync: Download Settings`

## Плагин для `Vs code`

### `Prettier` Красивое форматирование кода

- `>Prettier: Create Configuration File` Создаем настройки для форматирование кода
- `>Форматировать выбранное с помощью` Выбираем плагин для форматирования кода
- `>Форматировать документ` Форматируем документ (`Ctrl + Shift + I`)

Про настройки `.prettierrc` [+](https://prettier.io/docs/en/options.html):

- `tabWidth` Сколько пробелов
- `arrowParens` Ставить или нет круглые скобки для аргументов анонимных функций
- `bracketSpacing` Отступы до и после фигурных скобок
- `endOfLine` Как оканчивать документ
- `htmlWhitespaceSensitivity` Чувствительность к пробелам
- `jsxSingleQuote` Использовать одинарные кавычки вместо двойных (в JSX)
- `printWidth` Сколько символом разрешено в одной строке до принудительного переноса строки
- `semi` - Печать `;` в конце инструкции
- `singleQuote`- Использовать одинарные кавычки вместо двойных
- `trailingComma` - Когда ставить завершающие запятые
- `useTabs` Использовать `TAB` вместо пробелов
