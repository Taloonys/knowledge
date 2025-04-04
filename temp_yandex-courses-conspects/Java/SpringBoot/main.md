> Основной задачей `Spring Boot` является удобная инъекция объектов и их зависимостей. Т.е. это некоторый контейнер, в котором "поднимаются" определённые компоненты на основе некоторой конфигурации.

* Например, в других ЯП требуется объект нужного класса где-то инициализировать или вызвать конструктор, они явлются точками входа для такого подхода, в спринге точки входа напрямую могут не задаваться, они инициализируются аннотациями и самим спрингом.
* В Spring Boot это делается путём указания одного (или нескольких) аннотаций: `Component`, `Bean`, `Repository`, `Controller` и т.п. 
* Связывание в зависимости от уровня абстракций происходит по имени и с помощью аннотаций по типу `@Autowired`, `@Mapping` и т.п. 
* Входной точкой такого контейнера считается же `@SpringBootApplication`.

# Config stuff
* Есть 3 системы сборки: `Intellij build`, `maven`, `gradle`.
* `sdkman` позволяет удобно выбирать тип используемой для сборки проекта `JDK`
* `wrapper` позволяет при портирование проекта на другой ПК использовать maven для сборки с помощью скриптов (`mvnw`), не имея на этом ПК `maven`

# Dependency Injection
> При запуске SpringBootApplication контейнер спринга проходит по всем сущностям с аннотациями (по типу @Bean, @Serivce и т.п.) и "регистрирует" все эти компоненты и помещает их туда, где это требуется... это называется **Dependency Injection**.

* Список аннотаций обширный, можно ознакомиться тут: https://www.geeksforgeeks.org/spring-boot-annotations/