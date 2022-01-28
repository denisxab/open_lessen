# Установка `C#`

## в `Linux`

Мы будем устанавливать встроенную [версию](https://docs.microsoft.com/ru-ru/dotnet/core/install/linux-ubuntu)

```bash
wget https://packages.microsoft.com/config/ubuntu/21.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
rm packages-microsoft-prod.deb
```

Установка `SDK`

```bash
sudo apt-get update; \
  sudo apt-get install -y apt-transport-https && \
  sudo apt-get update && \
  sudo apt-get install -y dotnet-sdk-6.0
```

Проверим работоспособность

```bash
sudo dotnet --help
```

```c#
using System;

namespace my_pro
{
    class Prog
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello, World! +");
        }
    }
}
```

```bash
dotnet run $Файл$.cs
```

---

[Команды `dotnet`](https://docs.microsoft.com/ru-ru/dotnet/core/tools/dotnet)

| Команда                 | Описание                            |
| ----------------------- | ----------------------------------- |
| `dotnet new console`    | Создать новое консольное приложение |
| `dotnet run $Файл$.cs ` | Выполнить исходный код              |

## `Vs Code`

Плагины:

- [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)

---

Для того чтобы можно было использовать сборку проекта на уровне `VS code` нужно создать два файла

`.vscode/launch.json` (`program:` Путь может отличаться версией `net$.$`)

```json
{
	"version": "0.2.0",
	"configurations": [
		{
			"name": ".NET Core Launch (web)",
			"type": "coreclr",
			"request": "launch",
			"preLaunchTask": "build",
			"program": "${workspaceFolder}/bin/Debug/net6.0/${workspaceFolderBasename}.dll",
			"args": [],
			"cwd": "${workspaceFolder}",
			"stopAtEntry": false,
			"serverReadyAction": {
				"action": "openExternally",
				"pattern": "\\bNow listening on:\\s+(https?://\\S+)"
			},
			"env": {
				"ASPNETCORE_ENVIRONMENT": "Development"
			},
			"sourceFileMap": {
				"/Views": "${workspaceFolder}/Views"
			}
		},
		{
			"name": ".NET Core Attach",
			"type": "coreclr",
			"request": "attach"
		}
	]
}
```

`.vscode/tasks.json`

```json
{
	"version": "2.0.0",
	"tasks": [
		{
			"label": "build",
			"command": "dotnet",
			"type": "process",
			"args": [
				"build",
				"${workspaceFolder}/MyWebApp.csproj",
				"/property:GenerateFullPaths=true",
				"/consoleloggerparameters:NoSummary"
			],
			"problemMatcher": "$msCompile"
		},
		{
			"label": "publish",
			"command": "dotnet",
			"type": "process",
			"args": [
				"publish",
				"${workspaceFolder}/MyWebApp.csproj",
				"/property:GenerateFullPaths=true",
				"/consoleloggerparameters:NoSummary"
			],
			"problemMatcher": "$msCompile"
		},
		{
			"label": "watch",
			"command": "dotnet",
			"type": "process",
			"args": [
				"watch",
				"run",
				"${workspaceFolder}/MyWebApp.csproj",
				"/property:GenerateFullPaths=true",
				"/consoleloggerparameters:NoSummary"
			],
			"problemMatcher": "$msCompile"
		}
	]
}
```
