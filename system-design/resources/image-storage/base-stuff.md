## API

> **API (Application Programming Interface)** – это механизмы, которые позволяют **двум программным компонентам взаимодействовать друг с другом**, используя набор определений и протоколов. Например, система ПО метеослужбы содержит ежедневные данные о погоде. Приложение погоды на телефоне «общается» с этой системой через API и показывает ежедневные обновления погоды на телефоне.

## Монолитная архитектура

> Это традиционная модель создания программного продукта в виде **единого модуля**, который работает автономно и независимо от других приложений. 

Микросервисная архитектура противоположна монолитной, поскольку в данном случае архитектура организована в виде ряда независимо развертываемых сервисов.
Монолиты полезно использовать на начальных этапах проектов, чтобы облегчить развертывание и не тратить слишком много умственных усилий на управление кодом. 
Когда монолитное приложение становится большим и сложным, возникают трудности с его масштабированием и непрерывным развертыванием, а обновление становится неудобным.

### Pros
* Легко разрабатывать
* Легко дебажить (ну буквально сплошной код)
* Проект на монолите полезен, когда нужно быстро выкатить продукт
### Cons
* Сложно расширить
* С увеличением кодовой базы становится сложно поддерживать, т.к. надо помнить все зависимости
* Если чё т упало - то падает вообще весь проект
* При небольших правках - придётся в деплое замещать весь проект целиком

## Микросервисная архитектура

> В **микросервисной архитектуре** приложение разбивается на ряд независимо развертываемых сервисов, которые взаимодействуют с помощью **API-интерфейсов**. 

Довольно прикладное описание: https://www.youtube.com/watch?v=TQY4iL7CjuQ
### Pros
* Легко масштибировать конкретные места проекта
* Легко поддерживать код, т.к. всё декомпозированно
* Удобно поддерживать в cloud computing сервисах
* Удобно внедрять и деплоить новые фичи и/или сервисы
### Cons
* Сложно или почти невозможно дебажить
* Долгая реализация


![Untitled](resources/image-storage/Untitled%201.png)
