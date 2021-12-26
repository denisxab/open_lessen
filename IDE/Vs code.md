## Настройки для `Vs code`

- Настройке `settings.json`

    ```js
	{
		"workbench.colorTheme": "Default Dark+",
		"redhat.telemetry.enabled": true,

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
		"editor.mouseWheelZoom": true,

		"files.exclude": {
			"**/*.js": { "when": "$(basename).ts" },
			"**/**.js": { "when": "$(basename).tsx" }
		}

	}
    ```

- Все плагинов

    ```bash
    code --install-extension abusaidm.html-snippets
    code --install-extension batisteo.vscode-django
    code --install-extension CoenraadS.bracket-pair-colorizer-2
    code --install-extension dsznajder.es7-react-js-snippets
    code --install-extension ecmel.vscode-html-css
    code --install-extension esbenp.prettier-vscode
    code --install-extension k--kato.intellij-idea-keybindings
    code --install-extension ms-azuretools.vscode-docker
    code --install-extension MS-CEINTL.vscode-language-pack-ru
    code --install-extension redhat.vscode-yaml
    code --install-extension steoates.autoimport
    code --install-extension streetsidesoftware.code-spell-checker
    code --install-extension streetsidesoftware.code-spell-checker-russian
    code --install-extension streetsidesoftware.code-spell-checker-spanish
    code --install-extension Tomi.xasnippets
    code --install-extension xabikos.JavaScriptSnippets
    ```

    > Получить список плагинов. (В результате получаем текст для установки этих плагинов)
    >
    > ```bash
    > code --list-extensions | xargs -L 1 echo code --install-extension
    > ```

- `.prettierrc` - Для форматирования кода (плагин `Prettier`) [Prettier Красивое форматирование кода](Vs%20code.md#Prettier%20Красивое%20форматирование%20кода)

    ```json
    {
      "tabWidth": 2,
      "useTabs": true,
      "arrowParens":"always",
      "bracketSpacing":false,
      "endOfLine": "lf",
      "htmlWhitespaceSensitivity":"css",

      "printWidth":80,
      "proseWrap": "preserve",
      "quoteProps":"consistent",

      "semi": true,
      "jsxSingleQuote":true,
      "singleQuote":true,
      "trailingComma":"all"
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
