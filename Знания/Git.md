---
complete: Дописать Pychrm Git
data: 26.07.2021
---

## Общее

### Источники

[Скачать Git](https://git-scm.com/)

[Урок по Git 1](https://www.youtube.com/watch?v=zZBiln_2FhM)

[Урок по Git 2](https://www.youtube.com/watch?v=SEvR78OhGtw)

### Инициализация репозитория

```-
git int
```

### .gitignore

> Оформляться

```-
/Folder – вся папка игнорируеться
file.txt – файл игнорируеться
/Folder/file.png – вложенный файл игнорируретсья
```

> Потом нужно добавить .gitignore в отслеживание

```-
git add .gitignore
```

---

## Состояние репозитория

### Статус репозитория в ветке

```-
git status
```

<details><summary>Подробности вывода</summary>

> On branch master – **Имя ветки**

> No commits yet – **Количество комитов**

> Changes to be committed: - **Фалы, которые отлёживаются**

> Changes not staged for commit: **Файлы были изменены**

> Untracked files: - **файлы которые НЕ отслеживается**

</details>

### Посмотреть историю commit

```-
git log
```

![700x700](_attachments/bb0bd97d32842f72972d092d7c584085.png)

> Origin/master – **Commit на сервере**

> Head -> Name **Выбранный в данный момент commit**

> new_branch – **Ветки указывающие на commit**

### Показать конкретные изменения в ветки

```-
git diff
```

### Добавить файл в отслеживания

```-
git add <Nmae_File>
```

### Убрать файл из отслеживания

```-
git rm --cached <Name_File>
```

### Откатить ветку на прошлый commit

Откатить _**modified**_ файл на прошлый Commit

```-
git restore <NameFile>
```

Откатить _**committed**_ файл на прошлый Commit

```-
git restore --staged <NameFile>
```

## Работа с файлами

> При использовании этих команд происходит авто добавление в индекс

### Переместить файл

```-
git mv <new_file.txt> <Folder/>
```

### Переименовать файл

```-
git mv <new_file.txt> <NewFile.txt>
```

### Удалить файл

```-
git rm <NameFile.bmp>
```

- Удалить файл из отслеживания, но оставить его локально

    ```-
    git rm --cached <NameFile>
    ```

## Операции с Ветки

### Создать ветку

- Создать новую ветку

    ```-
    git branch <Name_Branch>
    ```

- Создать новую ветку и сразу переключиться на нее

    ```-
    git checkout -b <new_branch>
    ```

- Создать новую ветка на определенном коммите

    ```-
    git checkout -b <new_branch>
    ```

### Удалить ветку

- Удалить ветку – удалиться только если эта ветка слита с другой – **безопасное** удаление

    ```-
    git branch -d <test>
    ```

- Удалить ветку в **любом** случае
    ```-
    $ git branch -D <test>
    ```

### Навигация по веткам

- Посмотреть список веток

    ```$
    git branch
    	* master - Выбранная ветка
    	  test
    ```

- Переключиться на другую ветку

    ```$
    git checkout <test>
    ```

### Слияние веток Merge

1.  **Выбираем** ветку в которую нужно добавить код

    ```$
    git checkout <LastBranch>
    ```

2.  Соединяем **Измененную ветку** с, **Вибронной**

    ```$
    git merge <NewBranch>
    ```

### Commit

```-
git commit -am ”Сообщение о коммите”
```

### Push

```$
git push -u origin master
```

> origin - это новое имя удаленно сервера
>
> master – имя втеки удаленного сервера

### Pull

```$
git pull <origin> <master>
```

> origin - из сервера
>
> master – из ветки на сервере ветку

## Работа с GitHub

### Авторизоваться

> Нужно создать конфигурацию если ее нет

- Создать конфигурацию | Изменить

    ```$
    git config --global user.name “denis”
    git config --global user.email “denis@kustov.com”
    ```

- Посмотреть конфигурацию
    ```-
    git config user.name
    git config user.email
    ```

### Добавить данные о репозитории GitHub

Создать папку для синхронизации

```bush
git remote add origin <https://github.com/denisxab/TestGit.git>
```

> origin - любое имя папки
> ![Взять ссылку](_attachments/f56b30f876c96cf55fcec3333203f6e3.png)

Посмотреть Remote

```bush
git remote -v
```

> origin https://github.com/denisxab/TestGit.git (fetch) - для чтения
>
> origin https://github.com/denisxab/TestGit.git (push) - для записи

Заменить URL в `remote`

```bush
git remote set-url <origin> <url>
```

URL формируется по правилу

```bush
https://<user_name>:<token>@github.com/<user_name>/<Проект>
```

> `https://denisxab:<token>@github.com/denisxab/mydoc.git`

## Pychram Git

### Подготовка

- Включить контроль версий
    > ![](_attachments/9a36319400d50dfcc660e1327d42ff28.png)
- Создаем .gitignore для исключения пуша файлов
    > ![](_attachments/cc81f6c58a1e3e3835670dc7548a009f.png)
- Добавить отслеживание изменений фалов
    > ![](_attachments/3fa228f2f1b84cd250f2ad2d4f165381.png) > ![](_attachments/c50ef38b03fb27efb1984480dc070b7c.png) > ![](_attachments/09f3fae59c51ef7001e58f8072de8bac.png)
- Если надо, то войти в запись git
    > ![](_attachments/d5aa8b8a1feffc27944c2cd530aece4e.png)
- Добавляем ссылку репозитория

    > ![](_attachments/2907e6c62a5d03a0a46c8c72f4ddd84b.png)

- Получить URL и вводим
    > ![](_attachments/b267c89ebfeb31b624df8a5b14c159c0.png)
- Нужно создать ветку вспомогательную ветку

> ![](_attachments/c047b5f9a2a090ebaa76be7c409fa2f8.png)
>
>     После нужно пройти регистрацию на jlbreains
>     Потом на git создастся ветка master в которой будут писаться изменения

- Можно посмотреть изменения в файле
    > ![](_attachments/9e8298620bb32473286f7a353a2096ae.png)

### Слияние веток

1. Делаем commit на вторичную ветку

2. Делаем push на вторичную ветку

3. Выбираем первичную ветку

![](_attachments/3e69b73f2ba033657173ac7ffaa96931.png)

4. Делаем слияние со вторичной веткой

![](_attachments/81d07de5741a2f5d22686e7acc29ac67.png)

5. Делаем push первичной ветки

### Конфликт слияния веток

![](_attachments/9d060980fabcbcf13c8dc2969ca5edee.png)

>     Два раза нажимаем на файл
>     И выбрать какие значения принять в результате

### Посмотреть commit

![](_attachments/0dcd57a4d75291607c4220563c8fafea.png)

### Откатить ветку

![](_attachments/beca9d8ad9eb71d61ee3be397de3d29b.png)
![](_attachments/ffa7b52d8f01727c05e8aa309113b0e3.png)

### Принудительное слияние веток

```cmd
git merge master --allow-unrelated-histories
```
