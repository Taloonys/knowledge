* Не затенения переменных
```js
let value = 666;
let value = 2;    // error
```
# Строки
> Строки описываются всеми 3-мя способами ` `` `, `""`, `''`.
> Для интерполяции работает только ` `` ` и с помощью `${}`.

```js
let value = 5;
console.log(`value is ${value}`);
```
## IO text
> Document - это встроенный объект в браузере, который представляет собой текущую веб-страницу (HTML-документ).
```js
let input = prompt('1000 - 7?');            // отобразится окошко с текстом
                                            // input - строка*

console.log('any text');                    // отображается в консоли (на f12)
document.write('any text');                 // отображается на странице
document.write('<h1> any text <\h1>');      // отображается как элемент на странице
alert('any text');                          // отображается всплывающим окошком
```
# Function
```js 
function do_sth() {
	let input = prompt('1000 - 7?');
	alert('any text');
}

function sum(n1, n2) {
	return n1 + n2;
}

do_sth()
sum(1, 10);
```