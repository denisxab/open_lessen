Источники:

- [WSL На Windows 10| Установка И Настройка | Как Установить Linux В Windows 10](https://www.youtube.com/watch?v=NlObaul_XT4)

## Команды WSL

- Системы WSL

    ```-
    wsl --list
    ```

- Список запущенные образов в WSL

    ```-
    wsl --list --running
    ```

- Завершить запущенный образ (Linux)
    ```-
    wsl -t <NameWSl>
    ```

## Получить доступ к фалам Windows из WSL

```-
cd /mnt
```

## Устранение неполадок сети в WSL

[Если не работает интернет в WLS](https://coderoad.ru/62314789/Нет-подключения-к-Интернету-на-WSL-Ubuntu-подсистема-Windows-для-Linux)

### Подключить VS Code к WSL

> [Плагин Remote - WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)
