---
tags: [frontend]
aliases: [Оглавление]
---

# Оглавление
 * [Микрофронтенды](microfronts.md)
* [длинная отсылка к JavaScript](../code/languages/js.md)
* [React](react.md)
* Vue
# Base
## HTML (HyperText Markup Language)
> Язык разметки, формирующий то, какие элементы отображаются на странице, т.е. декларирует структуру страницы.
* Базовый пример html, который подключает скрипт на JS. 
* Есть одинарные (самозакрывающиеся) тэги, пример: `meta`, `scipt`.
* Есть парные тэги, пример: `head`, `body`.
* .. И нет, нельзя парные и одинарные тэги взаимозаменять
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>YourPageName</title>
</head>
<body>
    <script src='main.js'></script>
</body>
</html>
```
> **Тэги форматирования html** -> https://htmlbase.ru/article/html-tegi-dlya-oformleniya-teksta

Часто нужные (они все парные):
* `<b>` - Bold text,
* `<strong>` - Important text
* `<i>` - Italic text
* `<small>` - smaller text
* `<del>`  - marked as deleted ~~example~~
* `<u>` - mark text by line
* `<mark>` - mark text by color
* `<ul>` - from vertical list of element
* `<li>` - mark like an option (like that `*` in `.md`)
## CSS (Cascading Style Sheets) 
> Язык разметки, отвечающий за внешнее оформление элементов (цвета, margin и т.п.)
## JS (JavaScript)
> ЯП, который отвечает за "динамику страницы", иногда может в бэкенд логику... (бывает такое).
## Some concepts
### Ключевые файлы
> jsx ~ tsx -> первое для JavaScript, второй для TypeScript.
* `index.html` - часто является **страницей-входной точкой**, некоторые фреймворки (например, java spring boot) часто ищут это файл и отображают его при попытке попасть на сервис. Обычно лежит в корне проекта или в **public**.
* `index.css`, `styles.css` - тот же смысл, что и у `index.html`, но для вот... CSS....
* `main.jsx`, `index.jsx` - входная точка JS для приложения в экосистеме React (ибо `.jsx`)
* `App.jsx` - **главный компонент react** приложения.
* `package.json` - содержит информацию о пакетных зависимостях
* `*.confgig.js` - файл конфигурации для выбранной технологии
* 
# Techs
## Package managers
* `nodes.js`(`npm` - node package manager) - менеджер пакетов js. 
  Внутри него есть ещё `fnm`- это некоторого рода менеджер версий node.js.
* `Yarn`
* `Pnpm`
## Builders
* `vite` - на 2025г. довольно популярный сборщик, поддерживающий изначально большое кол-во модулей и быстрое обновление страницы при изменении кода.
* `CRA`(Create React App) - deprecated
* `Webpack`
## CSS-stylers
* Будем считать, что использование `vanilla css` имеет место быть.
* `Tailwindcss` - css-фреймворк, позволяющий задавать стили уже готовыми классами.
* `Bootstrap`
* `UnoCSS`
## Frameworks
* `React`
* `Vue`