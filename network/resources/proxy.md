> Промежуточный сервер-посредник, прокидывающий запросы пользователя на оригинальный сервер через себя. 
> Используется для ограничения доступа, может кэшировать запросы и данные по нему, повышает security и чуть ли не позволяет быть анонимным, ведь запрос на оригинальный сервер может идти от лица прокси.

* **Firewalls (WAFs)** - тоже прокси, но для локальных машин
* **SSL Offloading** - прокси-сервер TLS, который забирает на себя работу установление тунелей TLS + дешифровки и/или шифрования сообщений
# Типы
![Pasted image 20241027113432](image-storage/Pasted%20image%2020241027113432.png)

* **Forward proxy** - прокидывающий прокси, чаще всего только контролирует доступ
* **Reverse proxy** - что-то вроде распределяющего нагрузку прокси, что обычно находится "перед" несколькими серверами, используется для load balancing'а
* **Open proxy** - просто прокси для открытого использования, чаще всего используется для регионального обхода, прокси отправляет запросы пользователя от своего лица
* **Transparent proxy** - прокидывающий *насквозь* прокси, доступен с клиента, чаще всего используется для кэширования и фильтрации данных
* **Anonymous proxy** - прокси отправляет запросы на таргет сервер, исключая из запроса пользователя данные о нём (IP-адресс и т.п.)
* **Distorting proxy** - прокси отправляет запросы на таргет сервер, меняя IP-адресс пользователя и некоторые его данные на иные
* **High Anonymity proxy** - прокси, где какая-либо прямая или косвенная по максимому сокрыта или исключена