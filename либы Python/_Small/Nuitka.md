# `Nuitka`

## Полезные ссылки

- [Документация `Nuitka`](https://nuitka.net/doc/user-manual.html)

## Установка

Нужно иметь компилятор C++

| Linux | Windows |
| ----- | ------- |
| `gcc` | `mingw` |

> `mingw` это компилятор для Windows. После установки добавите путь к `mingw` в переменные окружения (`Path` = `C:\MinGW\bin`). Если вы все правильно сделали, то эта команда выведет версию компилятора `gcc --version`

---

Установить необходимые зависимости:

- `Arch`

    ```bush
    sudo pacman -S chrpath ccache
    ```

- `Ubuntu`

    ```bash
    sudo apt install patchelf python3.10-dev chrpath ccache
    ```

Установить `Nuitka`

```bush
pip install nuitka
```

## Компилляция Linux

Скомпилировать код в Исполнительный файл и создать папку с исходным кодом на `C`

```python
python -m nuitka --follow-imports <ФайлPytho>.py
```

Скомпилировать проект в один исполняемый файл `AppImage`

```bush
sudo python -m nuitka --onefile --show-scons --linux-onefile-icon=icons8-python-48.png <ФайлPytho>.py
```

## Компилляция Windows

## `Api`

| Флаг                               | Описание                                                          | Особенности                                                            |
| ---------------------------------- | ----------------------------------------------------------------- | ---------------------------------------------------------------------- |
| `--onefile`                        | Скомпилировать в один исполняемый файл                            | Предложит скачать `AppImage` нужно согласиться. Запускать через `sudo` |
| `--show-scons`                     | Вывести полный отчет о выполнение компиляции                      |                                                                        |
| `--linux-onefile-icon=Путь_к_фото` | Нужно для `--onefile`. Указать путь для иконки исполняемого файла | Фото должно быть формата `.png` `.svg`                                 |
| `--follow-import`                  | Будет также искать и компилировать файл которые вы импортируете   |                                                                        |
| `--standalone`                     | Переводит код в `C`, но не создает исполнительный файл            | Создаёт папку `<Имя>.dist`                                             |
| `--model`                          | Скомпилировать в `Python` модуль. (`.so`)                         | Можно `.so` импортировать так же как `.py`                             |
| `--python-flag=ФлагДляPython`      | Если вам нужно установить флаг для скомпилированного python       | Обычно можно использовать флаг `-O`                                    |
| `-o $НовоеИмя$`                    | Имя для скомпилированного файла                                   |                                                                        |

```txt
Опции:

  --module Создать исполняемый модуль расширения вместо программы. По умолчанию - off.

  --standalone	Включить автономный режим вывода. Это позволяет вам
				передавать созданный двоичный файл на другие машины без
				использования существующей установки Python. Это также
				означает, что он станет большим. Это подразумевает следующие опции: "--
				follow-imports". Вы также можете использовать "--python-
				flag=no_site", чтобы избежать использования модуля "site.py", который может
				сэкономить много зависимостей кода. По умолчанию установлено значение off.

  --onefile В дополнение к автономному режиму включите режим onefile.
  			Это означает, что не папка, а сжатый исполняемый файл
			создается и используется. По умолчанию выключено.

  --python-debug 	Использовать отладочную версию или нет.
  					По умолчанию используется та версия, которую вы
                    используете для запуска Nuitka, скорее всего, не отладочную версию.

  --python-flag=FLAG 	Использовать флаги Python. По умолчанию используется то, что вы используете для
                        запуска Nuitka, это принуждает к использованию определенного режима. Это
                        опции, которые также существуют для стандартного исполняемого файла Python.
                        В настоящее время поддерживаются: "-S" (псевдоним "no_site"),
                        "static_hashes" (не использовать рандомизацию хэшей),
                        "no_warnings" (не выдавать предупреждения во время выполнения Python),
                        "-O" (псевдоним "no_asserts"), "no_docstrings" (не использовать
                        docstrings), и "-m".  По умолчанию пусто.

  --python-for-scons=PATH 	Если используется Python3.3 или Python3.4, укажите путь к
							Python для Scons. В противном случае Nuitka может
							использовать то, с чем вы запускаете Nuitka, или двоичный файл "scons", который
							найденный в PATH, или установку Python из
							Реестр Windows.

  --warn-implicit-exceptions Включить предупреждения для неявных исключений, обнаруженных во время во время компиляции.

  --warn-unusual-code Включить предупреждения для необычного кода, обнаруженного во время компиляции. во время компиляции.

  --assume-yes-for-downloads	Разрешить Nuitka загружать внешний код при необходимости,
								например, dependency walker, ccache и даже gcc в Windows.
								Windows. Чтобы отключить, перенаправьте ввод с устройства nul,
								например, "</dev/null" или "<NUL:". По умолчанию используется запрос.

  Управлять включением модулей и пакетов в результат..:

    --include-package=PACKAGE	Включить весь пакет. Передайте как пространство имен Python,
								например, "some_package.sub_package", и Nuitka найдет его и включит в результат.
								найдет его и включит его и все модули, найденные ниже
								в этом дисковом пространстве в бинарный модуль или модуль расширения
								который она создает, и сделает его доступным для импорта в
								кодом. Чтобы избежать нежелательных подпакетов, например, тестов, вы можете
								можете, например, сделать так: "--nofollow-import-to=*.tests".
								По умолчанию пусто.

    --include-module=MODULE Включить отдельный модуль. Укажите в качестве пространства имен Python,
							например, "some_package.some_module", и Nuitka затем
							найдет его и включит в бинарный файл или расширение
							модуль, который она создает, и сделает его доступным для импорта в
							кодом. По умолчанию пусто.

    --include-plugin-directory=MODULE/PACKAGE	Включить содержимое этого каталога, независимо от того.
												он используется данной основной программой в видимой форме.
												Переопределяет все остальные опции включения. Может быть задан
												несколько раз. По умолчанию пустой.

    --include-plugin-files=PATTERN	Включить в файлы, соответствующие ПАТТЕРНУ. Отменяет все
									другие опции follow. Может быть задан несколько раз.
									По умолчанию пусто.

    --prefer-source-code 	Для уже скомпилированных модулей расширения, где есть
							и исходный файл, и модуль расширения, обычно
							используется модуль расширения, но лучше
							скомпилировать модуль из доступного исходного кода для достижения наилучшей производительности.
							лучшей производительности. Если это нежелательно, существует опция --no-
							prefer-source-code, чтобы отключить предупреждения об этом.
							По умолчанию выключено.

  Контролируйте следующее в импортируемых модулях:

    --follow-stdlib 	Также спускайтесь в импортируемые модули из стандартной
                        библиотеки. Это увеличит время компиляции на много. По умолчанию выключено.

    --nofollow-imports 	При использовании --nofollow-imports не спускаться в
                        какие-либо импортированные модули вообще, отменяя все другие
                        опции включения. По умолчанию выключено.

    --follow-imports 	Когда используется --follow-imports, попытка спуститься во
                        все импортированные модули. По умолчанию установлено значение off.


    --follow-import-to=MODULE/PACKAGE 	Следовать к этому модулю, если он используется, или, если это пакет, к
                        				всему пакету. Может быть задано несколько раз. По умолчанию пустой.

    --nofollow-import-to=MODULE/PACKAGE Не следовать к имени модуля, даже если он используется, или, если
										имя пакета, то ко всему пакету в любом случае,
										отменяет все остальные опции. Может быть задано несколько
										раз. По умолчанию пусто.

  Файлы данных для режима standalone/onefile:

    --include-package-data=PACKAGE
                        Включить файлы данных заданного имени пакета. Может использовать
                        шаблоны. По умолчанию Nuitka не включает их, если только они не закодированы жестко
                        и это необходимо для работы пакета. При этом
                        включает все не-DLL, нерасширяемые модули в
                        дистрибутиве. По умолчанию пусто.

    --include-data-file=DESC
                        Включить в дистрибутив файлы данных по именам файлов.
                        Существует множество допустимых форм. При использовании '--include-data-
                        file=/path/to/file/*.txt=folder_name/some.txt' будет
                        скопирует один файл и пожалуется, если их несколько. С
                        '--include-data-
                        file=/path/to/files/*.txt=folder_name/' он поместит
                        все совпадающие файлы в эту папку. Для рекурсивного
                        копирования есть форма с 3 значениями, которая '--include-
                        data-file=/path/to/scan=имя_папки=**/*.txt', которая
                        сохранит структуру каталога. По умолчанию пусто.

    --include-data-dir=DIRECTORY
                        Включить файлы данных из полного каталога в
                        дистрибутив. Это рекурсивная функция. Проверьте '--include-
                        data-file' с шаблонами, если вы хотите нерекурсивное
                        включение. Примером может быть '--include-data-
                        dir=/path/somedir=data/somedir" для простого копирования
                        всего каталога. Копируются все файлы, если вы хотите
                        исключить файлы, их необходимо предварительно удалить.
                        По умолчанию пусто.

  Немедленное выполнение после компиляции:

    --run Немедленно выполнить созданный двоичный файл (или импортировать скомпилированный модуль). По умолчанию выключено.

    --debugger, --gdb Выполнять внутри отладчика, например, "gdb" или "lldb", чтобы автоматически получить трассировку стека. По умолчанию выключено.

    --execute-with-pythonpath 	При немедленном выполнении созданного двоичного файла
								(--execute), не сбрасывайте PYTHONPATH. Когда все модули
								будут успешно включены, вам больше не понадобится
								PYTHONPATH больше не понадобится.


  Параметры дампа для внутреннего дерева:

    --xml Выгрузить конечный результат оптимизации в формате XML, затем выход.

  Варианты генерации кода:

    --full-compat 		Обеспечить абсолютную совместимость с CPython. Не
                        допускать даже незначительных отклонений от поведения CPython,
                        например, не иметь лучших трассировок или исключений
                        сообщения, которые на самом деле не являются несовместимыми, а только
                        отличаются. Это предназначено только для тестов и не должно
                        не следует использовать для обычного применения.

    --file-reference-choice=MODE
                        Выберите, каким будет значение "__file__". С
                        "runtime" (по умолчанию для автономного бинарного режима и
                        модульного режима), созданные двоичные файлы и модули используют
                        местоположение самих себя для вычитания значения
                        "__file__". Включенные пакеты делают вид, что они находятся в
                        каталогах ниже этого местоположения. Это позволяет вам
                        включать файлы данных в развертывания. Если вы просто стремитесь к
                        ускорения, вам лучше использовать значение
                        "исходное", при котором будет использоваться расположение исходных файлов
                        использоваться. При значении "замороженный" используется обозначение "<замороженный
                        имя_модуля>". По соображениям совместимости, значение
                        значение "__file__" всегда будет иметь суффикс ".py".
                        независимо от того, чем он является на самом деле.

	Варианты вывода:

    -o FILENAME Укажите, как должен быть назван исполняемый файл. Для
				модулей расширения нет выбора, также нет выбора для
				автономного режима, и его использование приведет к ошибке. Этот
				может включать информацию о пути, которая должна существовать
				хотя. По умолчанию для данной платформы используется '<имя_программы>'.bin

    --output-dir=DIRECTORY 	Укажите, куда следует поместить промежуточные и конечные выходные файлы
							должны быть помещены. В DIRECTORY будут помещены файлы на языке C
							файлы, объектные файлы и т.д. По умолчанию это текущий
							каталог.

    --remove-output Удаляет каталог сборки после создания модуля или exe-файла. По умолчанию off.

    --no-pyi-file 	Не создавать файл ".pyi" для модулей расширения
					созданных программой Nuitka. Это используется для обнаружения неявного
					импорта. По умолчанию выключено.


  Функции отладки:

    --debug Выполняет все возможные самопроверки для поиска ошибок в Nuitka. Nuitka, не используйте для производства. По умолчанию выключено.

    --unstripped Сохранять отладочную информацию в результирующем объектном файле для лучшего взаимодействия с отладчиком. По умолчанию выключено.

    --profile Включить профилирование затраченного времени на основе vmprof. Не работает в настоящее время. По умолчанию выключено.

    --internal-graph 	Создание графа внутренних процессов оптимизации, не следует использовать для целых программ,
						а только для небольших тестовых примеров. По умолчанию выключено.

    --trace-execution 	Вывод трассировки выполнения, вывод строки кода
                        перед ее выполнением. По умолчанию off.

    --recompile-c-only 	Это не инкрементальная компиляция, а для Nuitka
                        только для разработки. Берет существующие файлы и просто
                        компилирует их как C снова. Позволяет компилировать отредактированные C
                        файлы для быстрой отладки изменений в сгенерированном
                        исходного текста, например, чтобы увидеть, передается ли код, значения
                        выводить и т.д. По умолчанию установлено значение off. Зависит от компиляции
                        исходного текста Python, чтобы определить, какие файлы ему следует просматриватьaть.

    --generate-c-only 	Генерировать только исходный код на C, и не компилировать его в
                        двоичный код или модуль. Это необходимо для отладки и
                        анализ покрытия кода, который не расходует процессор. По умолчанию
                        off. Не думайте, что вы можете использовать это напрямую.

    --experimental=FLAG	Использовать функции, объявленные как 'экспериментальные'. Может не иметь
                        эффекта, если экспериментальные функции отсутствуют в
                        коде. Использует секретные теги (проверьте источник) для каждой экспериментальной
                        особенность.

    --low-memory 		Попытка использовать меньше памяти, форкнув меньше заданий компиляции C
                        компиляции и используя опции, которые используют меньше
                        памяти. Для использования на встроенных машинах. Используйте эту функцию в случае
                        проблем с нехваткой памяти. По умолчанию выключено.

  Выбор внутреннего компилятора C:

    --clang Обеспечить использование clang. На Windows для этого требуется
			рабочая версия Visual Studio для поддержки.
			По умолчанию выключено.

    --mingw64 Принудительное использование MinGW64 в Windows. По умолчанию off.

    --msvc=MSVC_VERSION
                        Обеспечить использование определенной версии MSVC в Windows.
                        Допустимые значения: например, "14.2" (MSVC 2019), указать
                        недопустимое значение для списка установленных компиляторов, или
                        использовать "latest". Обратите внимание, что только последняя версия MSVC действительно
                        поддерживается, и вы можете использовать "latest" для обеспечения этого.
                        По умолчанию используется MSVC on Windows, если он установлен,
                        иначе MinGW64.

    -j N, --jobs=N Укажите допустимое количество параллельных заданий компилятора Си
                   заданий. По умолчанию соответствует системному количеству CPU.

    --lto=choice Использовать оптимизации времени соединения (MSVC, gcc, clang).
                 Допустимые значения: "yes", "no" и "auto" (когда известно, что это
                 когда известно, что это работает). По умолчанию установлено значение "auto".


    --static-libpython=choice
                        Использовать статическую библиотеку Python. Допустимые значения
                        "yes", "no" и "auto" (когда известно, что она работает).
                        По умолчанию установлено значение "auto".

    --disable-ccache Не пытаться использовать ccache (gcc, clang и т.д.) или clcache (MSVC, clangcl).


 Варианты компиляции PGO:
    --pgo Включает оптимизацию профиля на уровне C (PGO), путем
                        сначала выполняется специальная сборка для профилирования,
                        а затем использует результат для обратной связи в компиляции C
                        компиляции. Примечание: Это экспериментальная функция, которая не
                        пока не работает с автономными режимами Nuitka. По умолчанию
                        выключено.
    --pgo-args=PGO_ARGS
                        Аргументы, которые будут передаваться в случае оптимизации с учетом профиля
                        оптимизации. Они передаются в специально созданный
                        исполняемому файлу во время выполнения профилирования PGO. По умолчанию
                        пустой.
    --pgo-executable=PGO_EXECUTABLE
                        Команда для выполнения при сборе
                        информации. Используйте это значение, только если вам нужно запустить его
                        через сценарий, который подготавливает ее к запуску. По умолчанию используется
                        созданная программа.

  Функции трассировки:
    --quiet Отключить вывод всей информации, но показывать предупреждения.
                        По умолчанию выключено.
    --show-scons Работает Scons в бесшумном режиме, показывая выполненные
                        команды. По умолчанию выключено.
    --show-progress Предоставление информации о ходе выполнения и статистики. По умолчанию
                        выключено.
    --no-progressbar Отключить вывод индикатора прогресса (если установлен tqdm).
                        По умолчанию выключено.
    --show-memory Предоставление информации о памяти и статистики. По умолчанию
                        off.
    --show-modules Предоставление информации о включенных модулях и библиотеках DLL.
                        По умолчанию выключено.
    --show-modules-output=PATH
                        Куда выводить --show-modules, должно быть имя файла.
                        По умолчанию - стандартный вывод.
    --report=COMPILATION_REPORT_FILENAME
                        Сообщить о включении модуля в выходной файл XML. По умолчанию
                        выключено.
    --verbose Выводить подробную информацию о предпринятых действиях, особенно при
                        оптимизации. Может быть очень много. По умолчанию выключено.
    --verbose-output=PATH
                        Куда выводить --verbose, должно быть имя файла.
                        По умолчанию - стандартный вывод.

  Элементы управления, специфичные для Linux:
    --linux-onefile-icon=ICON_PATH
                        Добавляет исполняемый значок для бинарного файла onefile, который будет использоваться. Может быть
                        задан только один раз. По умолчанию используется значок Python, если
                        доступен.

  Управление плагинами:
    --enable-plugin=PLUGIN_NAME, --plugin-enable=PLUGIN_NAME
                        Включенные плагины. Должны быть именами плагинов. Используйте --plugin-
                        list для запроса полного списка и выхода. По умолчанию пустой.

    --disable-plugin=PLUGIN_NAME, --plugin-disable=PLUGIN_NAME
                        Отключенные плагины. Должны быть именами плагинов. Используйте --plugin-
                        list для запроса полного списка и выхода. По умолчанию пусто.

    --plugin-no-detection
                        Плагины могут обнаружить, если они могут быть использованы, и вы
                        можно отключить предупреждение с помощью команды "--disable-plugin=plugin-
                        that-warned", или вы можете использовать эту опцию для отключения
                        механизм полностью, что также ускоряет
                        компиляцию, конечно, немного, так как этот код обнаружения
                        будет выполняться впустую, как только вы определитесь с тем, какие плагины
                        использовать. Значение по умолчанию - off.

    --plugin-list Показать список всех доступных плагинов и выйти. По умолчанию
                        выключено.

    --user-plugin=PATH Имя файла пользовательского плагина. Может быть задано несколько
                        раз. По умолчанию пусто.

    --persist-source-changes
                        Записывать исходные изменения в исходные файлы Python. Используйте
                        с осторожностью. Могут потребоваться разрешения, лучше всего использовать в
                        virtualenv для отладки того, работают ли изменения кода плагина с
                        стандартным Python или для получения выгоды от удаления раздутого кода даже
                        с чистым Python. По умолчанию False.
```

Win / Mac

```bash
  Windows specific controls:
    --windows-disable-console
                        When compiling for Windows, disable the console
                        window. Defaults to off.
    --windows-icon-from-ico=ICON_PATH
                        Add executable icon. Can be given multiple times for
                        different resolutions or files with multiple icons
                        inside. In the later case, you may also suffix with
                        #<n> where n is an integer index starting from 1,
                        specifying a specific icon to be included, and all
                        others to be ignored.
    --windows-icon-from-exe=ICON_EXE_PATH
                        Copy executable icons from this existing executable
                        (Windows only).
    --onefile-windows-splash-screen-image=SPLASH_SCREEN_IMAGE
                        When compiling for Windows and onefile, show this
                        while loading the application. Defaults to off.
    --windows-uac-admin
                        Request Windows User Control, to grant admin rights on
                        execution. (Windows only). Defaults to off.
    --windows-uac-uiaccess
                        Request Windows User Control, to enforce running from
                        a few folders only, remote desktop access. (Windows
                        only). Defaults to off.
    --windows-company-name=WINDOWS_COMPANY_NAME
                        Name of the company to use in Windows Version
                        information.  One of file or product version is
                        required, when a version resource needs to be added,
                        e.g. to specify product name, or company name.
                        Defaults to unused.
    --windows-product-name=WINDOWS_PRODUCT_NAME
                        Name of the product to use in Windows Version
                        information. Defaults to base filename of the binary.
    --windows-file-version=WINDOWS_FILE_VERSION
                        File version to use in Windows Version information.
                        Must be a sequence of up to 4 numbers, e.g. 1.0.0.0,
                        only this format is allowed. One of file or product
                        version is required, when a version resource needs to
                        be added, e.g. to specify product name, or company
                        name. Defaults to unused.
    --windows-product-version=WINDOWS_PRODUCT_VERSION
                        Product version to use in Windows Version information.
                        Must be a sequence of up to 4 numbers, e.g. 1.0.0.0,
                        only this format is allowed. One of file or product
                        version is required, when a version resource needs to
                        be added, e.g. to specify product name, or company
                        name. Defaults to unused.
    --windows-file-description=WINDOWS_FILE_DESCRIPTION
                        Description of the file use in Windows Version
                        information.  One of file or product version is
                        required, when a version resource needs to be added,
                        e.g. to specify product name, or company name.
                        Defaults to nonsense.
    --windows-onefile-tempdir-spec=ONEFILE_TEMPDIR_SPEC
                        Use this as a temporary folder. Defaults to
                        '%TEMP%\onefile_%PID%_%TIME%', i.e. system temporary
                        directory.
    --windows-force-stdout-spec=WINDOWS_FORCE_STDOUT_SPEC
                        Force standard output of the program to go to this
                        location. Useful for programs with disabled console
                        and programs using the Windows Services Plugin of
                        Nuitka. Defaults to not active, use e.g.
                        '%PROGRAM%.out.txt', i.e. file near your program.
    --windows-force-stderr-spec=WINDOWS_FORCE_STDERR_SPEC
                        Force standard error of the program to go to this
                        location. Useful for programs with disabled console
                        and programs using the Windows Services Plugin of
                        Nuitka. Defaults to not active, use e.g.
                        '%PROGRAM%.err.txt', i.e. file near your program.
  macOS specific controls:
    --macos-onefile-icon=ICON_PATH
                        Add executable icon for binary to use. Can be given
                        only one time. Defaults to Python icon if available.
    --macos-disable-console
                        When compiling for macOS, disable the console window
                        and create a GUI application. Defaults to off.
    --macos-create-app-bundle
                        When compiling for macOS, create a bundle rather than
                        a plain binary application. Currently experimental and
                        incomplete. Currently this is the only way to unlock
                        disabling of console.Defaults to off.
    --macos-signed-app-name=MACOS_SIGNED_APP_NAME
                        Name of the application to use for macOS signing.
                        Follow com.yourcompany.appname naming results for best
                        results, as these have to be globally unique, and will
                        grant protected API accesses.
    --macos-app-name=MACOS_APP_NAME
                        Name of the product to use in macOS bundle
                        information. Defaults to base filename of the binary.
    --macos-app-version=MACOS_APP_VERSION
                        Product version to use in macOS bundle information.
                        Defaults to 1.0 if not given.
```
