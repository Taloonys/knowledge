* Синтаксис, прям почти во всём С-подобный, поэтому каких-то уточнений по нему не будет.
* Не затенения переменных
```js
let value = 666;
let value = 2;    // error
```
* Есть сборщик мусора
# Data structures
* **Массивы** - по факту динамические `tuple`.
```js
let default_array = [1, 2, 3];
default_array.shift();         // 2 3
default_array.unshift(9);      // 9 2 3
default_array.push(9);         // 9 2 3 9

let tuple_like_array = [1, "text", false, { key: "value" }];
```
* **Objects** - это вот прям структура
```js
let obj = { id: 1, person: "Valera" };
alert(obj.person);
```
* **Map** - стандартная мапа, но в одной и той же мапе ключи могут быть любыми типами...
```js
let map = new Map();
map.set("name", "Alice");
map.set(1, "dummy");
```
* **Set** - вполне стандартный set
```js
let set = new Set([1, 2, 3, 3, 4]);   // set -> 1 2 3 4
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
# Functional
## Basic func
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
# Imports
```js
import defaultExport from "module-name";
import * as name from "module-name";
import { export1 } from "module-name";
import { export1 as alias1 } from "module-name";
```
## Стрелочные функции
> Аналог лямбд

```js
// let <name> = (<input arg/args>) => { <block> }
let summ = (a, b) => a + b;

let print_array = (arr) => arr.forEach(element => {
    document.write(`${element}<br>`);
});
```
# Conditions
* **Строгое равенство** `===` возвращает true, только если одиноковые типы и одиноковые значения
* **Нестрогое равенство** `==`, возвращает true, только если одинаковые значение в допустимых языком сценариях. Т.е. допускается неявный каст, например.
```js
var b1 = (5 == '5');        // true, строка с 5 скастится с 5
var b2 = (5 === '5');       // false, ибо разные типы
var b3 = (5 !== '5');       // true, как минимум -> разные типы
```
* Операторы `<`, `>` - **строгие**. 
* Операторы `<=`,`>=` - **нестрогие**.
# OOP
> Не чистое ООП, а **прототипное**!

Основное различие от обычного ООП в том, что вместо наследования для объекта некоторого класса задаётся **прототип** (поле **prototype**), которым уже указывается, условный, "родитель", и такой прототип, очевидно уже сконструирован, т.е. это больше похоже на композицию.
```js
let animal = { eats: true };
let rabbit = { jumps: true };
rabbit.__proto__ = animal;     // якобы наследование
console.log(rabbit.eats);      // true
```
Однако ничего не мешает делать как обычно:
```js
class Animal {

    constructor(name) {
        this.name = name;
    }
    
    speak() {
        console.log(`${this.name} makes a noise.`);
    }
}

class Dog extends Animal {

    speak() {
        console.log(`${this.name} barks.`);
    }
}

let dog = new Dog("Rex");
dog.speak();                   // "Rex barks."
```
# Async
* **Callback** - стандартный способ повесить на событие хэндлер
* **Promise** 
```js
let promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve("Done!"), 1000);
});
promise.then((result) => console.log(result)); // "Done!"
```
* **Async/await**
```js
async function fetchData() {
    let response = await fetch("https://api.example.com/data");
    let data = await response.json();
    
    console.log(data);
}
```