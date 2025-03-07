# Base
> **HyperText Transfer Protocol** - сетевой протокол прикладного уровня, изначально предназначенный для передачи HyperText Markup Language (**HTML**) файлов.
> "Изначально" - потому что сейчас HTTP используется в качестве "**транспорта**", а в своём теле могут передавать разного рода файлы, например, json в REST Api.

В голом виде HTTP запрос выглядит так:
```
GET /anydata HTTP/1.1  
Host: google.com
```
* `GET` - метод (здесь мы хотим получить данные)
* В первой строчке обязательно укзаывается версия HTTP протокола - `1.1`
* `/anydata` - **URI** (Uniform Resource Identifier) - унифицированный идентификатор ресурса, или некоторый путь до какого-то ресурса. 
Принимается запрос примерно в таком виде:
```
HTTP/1.1 200 OK
```
* 200 - **код состояния**
	Список всех кодов можно посмотреть тут -> https://ru.wikipedia.org/wiki/%D0%A1%D0%BF%D0%B8%D1%81%D0%BE%D0%BA_%D0%BA%D0%BE%D0%B4%D0%BE%D0%B2_%D1%81%D0%BE%D1%81%D1%82%D0%BE%D1%8F%D0%BD%D0%B8%D1%8F_HTTP
* OK - **Reason Phrase** - пояснение к нему
## Пример ответа с телом
* Через 2 строчки следует HTML'ем тело.
* Стащил пример отсюда: https://habr.com/ru/articles/215117/
```html
HTTP/1.1 302 Moved Temporarily
Server: nginx
Date: Sat, 08 Mar 2014 22:29:53 GMT
Content-Type: text/html
Content-Length: 154
Connection: keep-alive
Keep-Alive: timeout=25
Location: http://habrahabr.ru/users/alizar/


<html>
<head><title>302 Found</title></head>
<body bgcolor="white">
<center><h1>302 Found</h1></center>
<hr><center>nginx</center></body>
</html>
```
# Запросы
* https://www.w3schools.com/TAgs/ref_httpmethods.asp
* Ещё один пример всего описания: https://selectel.ru/blog/http-request/#:~:text=%D0%97%D0%B0%D0%BF%D1%80%D0%BE%D1%81%D1%8B%20(HTTP%20Requests)%20%E2%80%94%20%D1%81%D0%BE%D0%BE%D0%B1%D1%89%D0%B5%D0%BD%D0%B8%D1%8F,%D0%B2%20%D0%BE%D1%82%D0%B2%D0%B5%D1%82%20%D0%BD%D0%B0%20%D0%BA%D0%BB%D0%B8%D0%B5%D0%BD%D1%82%D1%81%D0%BA%D0%B8%D0%B9%20%D0%B7%D0%B0%D0%BF%D1%80%D0%BE%D1%81.