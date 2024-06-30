[[FrontEnd]]

```
// инициализация приложения в корневой папке фронтэнда
npm init
```
- После будет создан package.json
```
// уствновка webpack
// -D означать только во время разработки
npm i -D webpack webpack-cli
```
- Добавим команду для сборки
```json
"scripts": {
    "build": "webpack --mode production",
    "dev": "webpack --mode development"

  },
```
- Запуск
```
npm run build
```
- Создаем webpack.config.js
```js
const path = require("path")
module.exports = {
// production - супер сжатые файлы
   mode: "development || production"
// entry - главный файл
// теперь в index.js можнр использовать import
    entry: "./src/js/index.js",
    output: {
    // сжатый файл
        filename: "bundle.js",
   // путь для сжатого файла
        path: path.resolve(__dirname,"dist")
    }
}
```
- Автоматический hash
```js
output:{
	filename:"bundle.[contenthash].js"
}
```
- Сборка html
```
 npm i -D html-webpack-plugin
```
- Добавление
```js
const HtmlWebpackPlugin = require("html-webpack-plugin")
// в module.exports
plugins:[
	new HtmlWebpackPlugin({
		// путь к html
       template: "./src/index.html"
    })
]
```
- Удаление ненужных файлов (со старым hash)
```
npm i -D clean-webpack-plugin
```
- Подключение
```js
const {CleanWebpackPlugin} = require("clean-webpack-plugin")
// ...
plugins:[
	new CleanWebpackPlugin()
]
```
- Контекст указывает начальный путь (src)
```js
module.exports = {
    mode: "production",
    // начальный путь
    context : path.resolve(__dirname,"src"),
    // в папке начального пути
    entry: "./index.js",
    output: {
	    // здесь уже абсолютный путь
        filename: "[name].[contenthash].js",
        path: path.resolve(__dirname,"dist")
    },
    plugins:[
        new HtmlWebpackPlugin({
            template: "./src/index.html"
        }),
        new CleanWebpackPlugin()
    ]
}
```
- Добавление стилей на страницу
```js
// index.js
import "../styles/styles.css"
```
- Подключение
```js
//webpack.confing.js
module:{
        rules:[
            {
            // расширение файла(регулярка)
                test: /\.css/,
            // loader
                use: ["style-loader","css-loader"]
            }
        ]
    }
```
```
npm i css-loader style-loader
```
- Работа с файлами
```
npm i -D file-loader
```
```js
//webpack.config.js
{
    test:/\.(png|jpg|svg)$/,
    use: ["file-loader"]
}
```
- Теперь можно импортировать картинки
```js
import Web from "../favicon.png"
// web хранит url будущего сжатого png файла
```
- В css
```css
// указываем не сжатый файл (src)
.logo{
   background-image: url("../src/favicon.png")
}
```
- Доп настройка
```js
resolve:{
// автоматическое нахождение файла в импорте post = post.js
// по умолчанию js json
	extensions: [".js",".json","png"]
	alias:{
	// сокращение путей в импортах
	  "@model" : path.resolve(__dirname,"/daf/dsf/dsfd/ds/fds/fsad")
	}
}
```
```js
// extensions
import Post from "Post"
// alias
import Service from "@model/Service"
```
- Подключение js библиотек
```
npm i jquery
```
```js
import * as $ from "jquery"
```
- Оптимизация (для использования библиотек в нескольких файлах)
```js
optimization:{
        splitChunks:{
            chunks: "all"
        }
    },
```
- Использование статических файлов в html
```
npm i -D copy-webpack-plugin
```
```js
const CopyWebpackPlugin = require("copy-webpack-plugin")
// ... 
plugins:[
	new CopyWebpackPlugin({
		patterns:[
		 {
			 from: path.resolve(__dirname,"src/images"),
			 to: path.resolve(__dirname,"dist/images")
		 }
		]
	})
]
```
- css (в отдельный файл)
```
npm i -D mini-css-extract-plugin 
npm i css-minimizer-webpack-plugin -D
```
```js
const MiniCssExtractPlugin = require("mini-css-extract-plugin")
// меняем style-loader на min-css
// в rules
{
     test: /\.css/,
     use: [MiniCssExtractPlugin.loader/*("style-loader")*/,"css-loader"]
},
// в плагинах 
new MiniCssExtractPlugin({
            filename: "main.[hash].css"
 })
```
- Сжатие html 
```js
new HtmlWebpackPlugin({
       template: "index.html",
        minify:{
            collapseWhitespace: true
        }
}),
```
- Сжатие css
```
```console
npm install css-minimizer-webpack-plugin --save-dev
```

```js
const CssMinimizerPlugin = require("css-minimizer-webpack-plugin");
// ... 
use: [MiniCssExtractPlugin.loader, "css-loader", "sass-loader"],
// ... 
new MiniCssExteractPlugin()
```


- Sass
```
npm i -D node-sass sass-loader
```
```js
import "../styles/styles.scss"
```
```js
//webpack.config.js
{
                test:/\.s[ac]ss$/,
                use:["style-loader","css-loader","sass-loader"]
}
```