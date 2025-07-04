# Domain-Driven Design (DDD)
> Функциональная ответственность ~= домен...
> Соответственно **DDD** - это про методология, в которой многое отталкивается именно от доменов.
* **Домен** - область знаний или деятельности
  *Часть компании, отвечающая за поиск сотрудников (HR) - домен*
* **Поддомен** - логическая часть домена, больше похоже на конкретную функцию
  *Отбор нужных анкет соискателей - возможная функция внутри HR*
* **Контекст** - область, где конкретные термины имеют конкретное значение
  *"Разбить стекло" и "разбить сервис" в разных контекстах - разные значения :3*
## Стратегическое проектирование
> Это уровень проектирования, на котором определяется общий вид системы и взаимодействие между поддоменами + предоставить контекст
* **Bounded context** - определение границ того, что является контекстом
* **Context map** - описание-диаграмма с взаимодействием контекстов
## Тактическое проектирование
> Это уровень проектирования, который уже описывает детали каждого контекста
* **Entities** - ключевые сущности
  *Например, лента новостей и пользователь*
* **Value Objects** - значимые объекты + могут быть со своими атрибутами
  *Например, лайки*
* **Aggregates** - группы объектов, которые можно рассматривать как нечто целое
  *Например, посты*
* **Repositories** - интерфейсы для хранилищ данных (наполненных агрегатами)
  *Например, репозиторий или JPA для БД с постами*
* **Services** - в некотором смысле вспомогательные и независимые "сущности"
  *Например, сервис рекомендаций*
## Процесс выделения DDD
* Анализ бизнес требований
* Разделение на домены
* Разделение на поддомены
* Разделение на контексты
* Построение Context Map
* Итеративное уточнение
# Patterns
* [Patterns for microservices](microservices-patterns.md)
# Visualization tools
## Diagrams
* UML
* C4 - https://c4model.com/
* Archimate
* BPMN
* ERD
* Structurizr
* Ad-hoc
## Tools
* PlantUML - текст превращается в диаграммы, работает как плагин для многих IDE
* MKDocs - python утилита для развёртывания диаграм и документации в браузере