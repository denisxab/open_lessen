---
date created: 2021-12-26 18:09
date updated: 2021-12-26 18:10
---

# Webpack

- `webpack` - Компилятор `JS`
- `webpack-cli` - Консольные команды для `webpack`
- `webpack-dev-server` - Лайф сервер
- `html-webpack-plugin` - Подключать актуальные версии `TS` скриптов (это нужно когда имена скриптов генерируются на основе своего хеше)
- `clean-webpack-plugin` - Удалять ненужные старые версии из сборки `output`
- `css-loader` - Для возможности импортировать `.css` в `js`
- `style-loader` - Автоматически подключает `.css` в `HTML`
- `file-loader` - Для возможности импортировать файлы `png|jpg|svg|gif|web`
- `copy-webpack-plugin`- Копировать файлы при сборки проекта
- `terser-webpack-plugin`
- `optimize-css-assets-webpack-plugin`
- `mini-css-extract-plugin` - Вынести стили `css` в отдельный файл и подключить их к `html`
- `terser-webpack-plugin` - Сжатие `JS`
- `optimize-css-assets-webpack-plugin` - Сжатие `CSS`
- `webpack-bundle-analyzer` - Анализ сборки

---

- `sass` - Ядро `SCSS`
- `sass-loader` - Для возможности импорта файлов `.scss`

---

## Конфигурация для `React+Django`

`package.json`

```json
{
  "name": "{{ project_name }}",
  "version": "1.0.0",
  "description": "...",
  "private":true,
  "scripts": {
    "/1": "Запустить автоматически перезагружаемый сервер [--mode development]",
    "dev": "webpack-dev-server --config {{ project_name }}/frontend_react/webpack.config.js",
    "/2": "Собрать проект `JS`. Для оптимизированной сборки измените `__env.env->DEBUG='false'` или [--mode production]",
    "build": "webpack --config {{ project_name }}/frontend_react/webpack.config.js",
    "/3": "Получить данные о размере банглов",
    "status": "webpack --config {{ project_name }}/frontend_react/webpack.config.js --json >status.json && webpack-bundle-analyzer status.json"
  },
  "author": "...",
  "license": "ISC",
  "devDependencies": {
    "@types/react": "^16.8.24",
    "@types/react-dom": "^16.0.5",
    "@types/webpack": "4.1.4",
    "clean-webpack-plugin": "^4.0.0",
    "copy-webpack-plugin": "^10.2.0",
    "css-loader": "^6.5.1",
    "file-loader": "^6.2.0",
    "html-webpack-plugin": "^5.5.0",
    "mini-css-extract-plugin": "^2.4.5",
    "optimize-css-assets-webpack-plugin": "^6.0.1",
    "sass": "^1.45.0",
    "sass-loader": "^12.4.0",
    "style-loader": "^3.3.1",
    "terser-webpack-plugin": "^5.3.0",
    "ts-loader": "^6.2.1",
    "typescript": "^3.9.10",
    "webpack": "^5.65.0",
    "webpack-bundle-analyzer": "^4.5.0",
    "webpack-cli": "^4.9.1",
    "webpack-dev-server": "^4.6.0"
  },
  "dependencies": {
    "bootstrap": "^5.1.3",
    "react-bootstrap": "^2.0.4",
    "dotenv": "^10.0.0",
    "react": "^16.12.0",
    "react-dom": "^16.12.0",
    "react-router-dom": "^6.2.1"
  }
}
```

`webpack.config.js`

```json
// Для относительных путей
const path = require("path");
// https://github.com/jantimon/html-webpack-plugin#options
const HTMLWebpackPlugin = require("html-webpack-plugin")
// https://github.com/johnagan/clean-webpack-plugin
const {CleanWebpackPlugin} = require('clean-webpack-plugin')
// https://www.npmjs.com/package/copy-webpack-plugin
const CopyWebpackPlugin = require('copy-webpack-plugin')
// https://webpack.js.org/plugins/mini-css-extract-plugin/
const MiniCssExtractPlugin = require('mini-css-extract-plugin')
// https://www.npmjs.com/package/optimize-css-assets-webpack-plugin
const OptimizeCssAssetsPlugin = require('optimize-css-assets-webpack-plugin')
// https://webpack.js.org/plugins/terser-webpack-plugin/
const TerserPlugin = require('terser-webpack-plugin')
// https://www.npmjs.com/package/webpack-bundle-analyzer
const {BundleAnalyzerPlugin} = require('webpack-bundle-analyzer')

// Считываем переменные окружения из файла `npm install dotenv`
const envy = require("dotenv").config({path: "./__env.env"});
//Получаем имя проекта из переменных окружения
const ProjName = envy.parsed.NAME_PROJ;
console.log("ProjName:\t", ProjName);
// Получить режим разработки (bool)
const isDev = envy.parsed.DEBUG === 'true'
console.log("isDev:\t\t", isDev)
const strDev = isDev ? 'development' : 'production'
console.log("DEBUG:\t\t", strDev)


// Функция для настроек оптимизации (сжатия) файлов
const optimization = () => {
    // Оптимизировать импорты сторонних библиотек
    const conf = {
        splitChunks: {
            chunks: "all"
        }
    }
    // Если не режим разработки то сжимаем `JS` и `CSS`
    if (!isDev) {
        conf.minimize = true
        conf.minimizer = [
            new OptimizeCssAssetsPlugin(),
            new TerserPlugin()
        ]
    }
    return conf
}

// Нужно ли создавать хешь в имени файлов. Не нужно в режиме разработки
const filename = ext => isDev ? `[name].bundle.${ext}` : `[name].[contenthash].${ext}`

// Функция для подключения плагинов
const plugins = () => {
    plug = [
        // Плагин для автоматического подключения актуальной
        // версии `TS` когда в `index.html`
        new HTMLWebpackPlugin({
            // Какой `HTML` шаблон взять за основу
            template: path.resolve(__dirname, `templates/frontend_react/index.template.html`), // Куда поместить итоговый `HTMl` файл
            filename: path.resolve(__dirname, `templates/frontend_react/index.html`), // Оптемезировать сборки `HTMl` если не режим разработки
            minify: {
                // Варианты: https://github.com/terser/html-minifier-terser#options-quick-reference
                collapseWhitespace: !isDev,
                keepClosingSlash: !isDev,
                removeComments: !isDev,
                removeRedundantAttributes: !isDev,
                removeScriptTypeAttributes: !isDev,
                removeStyleLinkTypeAttributes: !isDev,
                useShortDoctype: !isDev
            }
        }),
        // Удалять старые версии скриптов из `output`
        new CleanWebpackPlugin(),
        // Создать общий `.css` файл со стилями
        new MiniCssExtractPlugin({
            filename: filename('css'),
        })
        // // Копировать файлы или папки при сборки проекта
        // new CopyWebpackPlugin([
        //         // Копирование
        //         {
        //             // Откуда копировать
        //             from: path.resolve(__dirname,``),
        //             // Куда копировать
        //             to: path.resolve(__dirname,``)
        //         },
        //     ]
        // )
    ]
    if (!isDev) {
        plug.push(new BundleAnalyzerPlugin())
    }

    return plug
}

// https://webpack.js.org/guides/typescript/
module.exports = {
    // Режим работы [production(сжатие кода)/development]
    mode: strDev,


    // Вариант сборки https://webpack.js.org/configuration/devtool/
    devtool: isDev ? "source-map" : false,


    // Выходные файл приложения
    entry: {
        // Его мы подключаем в `index.html`
        main: path.resolve(__dirname, `src/index.tsx`)
        // Путь к другому файлу для компиляции
        // other: path.resolve(__dirname, `src/other.tsx`)
    },


    // Выходные файлы компиляции
    output: {
        // Имя выходного файла.
        // `name` возьмётся из ключа `entry`.
        // `contenthash` будет создавать хеш файла для индивидуальности
        filename: filename('js'),

        // куда помещаются скомпилированные файлы
        path: path.resolve(__dirname, `static/frontend_react/public/`), // 127.0.0.1/static/frontend_react/public/ откуда подаются файлы
        publicPath: "/static/frontend_react/public/",
    },


    // Оптимизировать импорты сторонних библиотек
    optimization: optimization(),


    // Файлы с каким расширением мы подключаем без указания расширения
    resolve: {
        extensions: [".ts", ".tsx", ".js"],
    },


    // Список используемых плагинов
    plugins: plugins(),

    // Настройки для различных форматов файлов (предпроцессоры)
    module: {
        // конфигурация относительно модулей
        rules: [
            // TS
            {
                // свойство определяет, какой файл или файлы следует преобразовать.
                test: /\.tsx|ts?$/,
                // Игнорирует папки [node_modules/, bower_components/]
                exclude: /(node_modules|bower_components)/,
                // какой загрузчик следует использовать для преобразования.
                use: "ts-loader",
            },
            // CSS
            {
                test: /\.css$/,
                // `css-loader` - поваляет импортировать `.css` в `js`
                // `style-loader` - подключает  `.css` в `HTML` (Удален)
                // `MiniCssExtractPlugin` -  создавать отдельный `.css` файл
                use: [// Настройки для `MiniCssExtractPlugin`
                    {
                        loader: MiniCssExtractPlugin.loader, options: {}
                    }, "css-loader"]
            },
            // SASS-SCSS
            {
                test: /\.s[ac]ss$/,
                // `css-loader` - поваляет импортировать `.css` в `js`
                // `style-loader` - подключает  `.css` в `HTML` (Удален)
                // `MiniCssExtractPlugin` -  создавать отдельный `.css` файл
                use: [// Настройки для `MiniCssExtractPlugin`
                    {
                        loader: MiniCssExtractPlugin.loader, options: {}
                    },
                    "css-loader",
                    "sass-loader"
                ]
            },
            // File
            {
                test: /\.(png|jpg|svg|gif|web)$/, use: ['file-loader']
            },
            // Fonts
            {
                test: /\.(ttf|woff|woff2|eot)$/, use: ['file-loader']
            }],
    },

    // Для кеширования
    // externals: {
    //     "react": "React",
    //     "react-dom": "ReactDOM"
    // },

    // Настройка `webpack-dev-server`
    devServer: {
        // Порт на котором будет запущен Лайф сервер
        port: 8011, devMiddleware: {
            // Записывать изменения в файл, а не в ОЗУ
            writeToDisk: true,
        },
        // Атоперезагрузка если режим разработки
        hot: isDev
    },
};
```
