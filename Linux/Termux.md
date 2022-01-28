# Установка Ubuntu без рут

[Исходный гайд](https://tech-geek.ru/install-ubuntu-on-android-termux/)

---

1. Обновить систему

    ```bash
    apt update; apt upgrade;
    ```

1. Скачать установщик

    ```bash
    apt install proot-distro
    ```

1. Установить `ubuntu`

    ```bash
    proot-distro install ubuntu
    ```

1. Войти в `ubuntu`

    ```bash
    proot-distro login ubuntu
    ```

> -   Создать файл для запуска
>     ```bash
>     "proot-distro login ubuntu" > ubu.sh
>     ```
