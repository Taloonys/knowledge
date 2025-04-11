# Base
* Мультпарадигменный
* Духовный наследник **хаскеля** во многих моментах
* **ООП как в GO** -> есть структура и отдельно к ней можно применить совокупность методов, но в расте это помощнее + более гибкое
* Помимо простого навешивания методов на структуру, есть ещё отдельные "черты" -> **traits**
* Rust analyzer - strong
* Любое использование значений означает "**семантику передачи владения**" (оно же "перемещение"), копирование происходит только во время явных вызовов `copy()`,`clone()` и т.п.
* Компилятор **rustc** (есть как **msvc** версия, так и **gcc**). MSVC - винда, GCC - linux
* **cargo** - пакетный менеджер + система сборки
* Есть **аналог аннотациям и декораторам** - **макросы** (они не как в плюсах!), функции с `!` в конце тоже макросы*
* Запрещены неявные касты
* Тут метапрограммирование плотно завязано на трейтах и дженериках.
* Документации: 
	* (ru) https://doc.rust-lang.ru/stable/nomicon/intro.html 
	* (en) https://doc.rust-lang.org/nomicon/
* быстро по синтаксису: https://github.com/Taloonys/languages-basic-syntax/tree/main/rust
# Files & scopes
## Crate
>Минимальная единица компиляции, lib.rs - библиотечный crate, main.rs - исполняемый крейт
### 2018+
* Внешние crate'ы по умолчанию видны всему проекту
### before 2018
* Внешние crate'ы приходится подключать через `extern crate <name>;`
## Package 
>Условно одному Cargo.toml соответствует один пакет
### build configuration
>Файл `build.rs` в root директории растом считается описателем сборки, в `Cargo.toml` он связан с категорией `[build-dependencies]`.
```rust 
// build rs

/* Указание того, где будет поиск файлов и каких (оно же и распечатается) */
fn main() {
	// ...
	println!("cargo:rustc-link-search={search_dir}",
			 search_dir = ANY_DIR);
	println!("cargo:rustc-link-lib={lib_name}", 
			 lib_name = ANY_API_BIN_NAME);
}
```
## Module 
>Каждый файл по умолчанию является модулем (название файла совпадает с именем его модуля)
* Хочется использовать соседний `.rs` файл в том же крупном модуле?
```rust
//another_nested.rs

use super::nested_package::AnyType;

let x: AnyType = ...;
```
### 2018+
```
src
├── my_package
│   ├── nested_package.rs
│   └── another_nested.rs
├── my_package.rs
└── main.rs
```
* Для того, чтобы внутренние модули были видны из-вне:
```rust
// my_package.rs
pub mod nested_package;
pub mod another_nested;
```
### before 2018
Грубо говоря `pub mod` переехали в `mod.rs`, который является входной точкой или описателем модуля `my_package`
```
src
├── my_package
│   ├── nested_package.rs
│   ├── another_nested.rs
│   └── mod.rs
└── main.rs
```
# Testing
>Тесты собираются отдельными крейтами, поэтому зависимости билда в них уже не работают
# Base
## Operator ?
>Укороченная обработка "проброса" ошибки или результата дальше
```rust 
fn transfer_string() -> Result<String, i32> {
	get_string()?                              // внимание на оператор ?
}

// равносильно:

fn transfer_string() -> Result<String, i32> {
	match get_string() {
		Result::Ok { String, _ } => Ok(String),
		Result::Error { _, i32 } => Error(i32)
	}
}
```