* link -> https://habr.com/ru/articles/804323/, многие примеры и темы с этого источника
* для терминологии: **registry** - грубо говоря хаб, откуда выкачиваются образы или иного рода ресурсы.
# Base
> Система контейнеризации приложения.
* **VM (Virtaul Machine)** содержит одно ядро (или операционную систему) и поверх него работают гостевые операционные системы, они уже управляются гипервизором.
* Docker же располагает поверх операционной системы и содержит в себе контейнеры.
![](image-storage/Pasted%20image%2020250309180207.png)
* **Digest** - уникальный хэшкод образа
## Image
> Набор средств для развёртывания контейнера.
* Либо описывается `Dockerfile`, а потом его собирают (`docker build`)
* Выкачивают образ с интернета (dockerhub) и уже его запускают (`docker run`)
* `docker image ls` - для списка всех образов
## Container
> Собранный по некоторому образу контейнер.
* Можно лишь запустить (`docker run`)
* `docker ps` - для списка всех контейнеров
* У контейнеров должны быть ещё уникальные (на момент runtime'а) имена
* В некоторых случаях контейнеры могут не стартовать, но при это создаться (например порт занят другий контейнером)
* `docker exec -it <container_id> sh` - чтобы залезть в терминал контейнера
* `apk add --no-cache <утилита>` - скачать какую-нибудь нужную утилиту прям в запущенном контейнере 
* `nslookup <домен, например backend>` - проверить, что сеть докера (днс) резолвит правильно имя
* `ping <домен>` - примитивная проверка доступности
# Dockerfile
> В нём описываются этапы построения образа OverlayFS
## Команды
> https://docs.docker.com/reference/dockerfile/
> Из важного: FORM, WORKDIR, COPY, RUN, EXPOSE, ARG, ENV, VOLUME
## image build logic
> Докер использует собственную файловую систему **OverlayFS** (https://habr.com/ru/companies/flant/articles/862252/).
> А также команды *dockerfile* `COPY`, `ADD`, `RUN` (буду называть их "слой-команды") добавляют абстрактные слои сборки. 
> Каждый последующий слой отражает изменения (написанные в докерфайл) относительно предыдущего слоя.
> Относительно всего этого есть некоторые рекомендации...
* Стараться лишние файлы удалять как можно раньше до "слой команд"
```dockerfile
FROM ubuntu

# 1st layer
COPY weird.txt        

# 2nd layer, здесь weird.txt добавит слою пару байт
RUN ...               

# 3rd layer, хотя txt файл мог удалить до 2 слоя...
RUN rm weird.txt      
```
* Некоторые установки через пакетные менеджеры линух (например `apt get`) также куда-то копируют временные файлы, стоит их удалять в той же RUN команде (добавить команду `&&`, а перенос строки `\`)
```dockerfile
...
RUN apt-get update && \
    apt-get install -y nginx && \
	rm -f /var/lib/apt/lists/* && \
	apt get pip
```
* **dockerignore** порой бывает полезен...
* При пересборке докер может неизменные слои кэшировать и переиспользовать, но это не всегда возможно. Например `COPY . .` не даёт чёткого понимания, кем сейчас может быть текущая директория, это не будет кэшироваться.
* Стоит разделять сборку проекта и запуск проекта
```dockerfile
FROM maven:3.8-openjdk-17 AS build
COPY /src /src
COPY pom.xml /
RUN mvn -f /pom.xml clean package

# В слое выше мы скомпилировали application.jar файл
# Новым слоем делаем среду, в которой только этот файл и есть
# Т.е в одном слое собрали, в другом утилит для сборки уже не будет

FROM openjdk:17-jdk-slim                          
COPY --from=build /target/*.jar application.jar  
EXPOSE 8083
ENTRYPOINT ["java", "-jar", "application.jar"]
```
# compose
> Утилита, позволяющая собирать и запускать одновременно несколько контейнеров.

**Понятный пример:** 
(стащил отсюда https://github.com/SuchkovDenis/docker-compose-demo/tree/master)
```yaml
# version: "3.8"                             # В старых версиях надо указывать

services:

  backend:                                   # 1. именуем наш сервис с бэкендом
    image: shop_backend                      # такое будет будущее имя контейнера
    build:                                  
      context: ./backend                     # указываем в какой папке искать Dockerfile
    depends_on:                              # указываем ПОСЛЕ кого запускаемся
      db:
        condition: service_healthy           # ну и на каких условиях (см db)
      search:
        condition: service_healthy           # то же, что и с бд
    environment:                             # указываем переменные окружения, которыми сервис будет пользоваться
      SPRING_DATASOURCE_URL: jdbc:postgresql://db:5432/${DB_NAME}
      SPRING_DATASOURCE_USERNAME: ${DB_USER}
      SPRING_DATASOURCE_PASSWORD: ${DB_PASSWORD}
      SPRING_ELASTICSEARCH_URIS: http://search:9200
	expose:
	  - "8081"                               # открыть порт для прослушивания
	  
  frontend:                                  # 2. именуем сервис с фронтэндом
    image: shop_frontend
    build:
      context: ./frontend
    ports:
      - "80:80"                              # маппится <наш порт> : <порт контейнера>
    environment:
      BACKEND_HOST: backend
      BACKEND_PORT: 8080
    depends_on:
      - backend
	  
  db:                                        # 3. именуем сервис с БД
    image: postgres:16.0                     # * тут указываем существующий в интернете образ, он будет запуллен
    environment:
      POSTGRES_PASSWORD: ${DB_PASSWORD}
      POSTGRES_USER: ${DB_USER}
      POSTGRES_DB: ${DB_NAME}
    healthcheck:                            # способ проверки "жив ли сейчас сервис"
      test: ["CMD-SHELL", "pg_isready -U $${POSTGRES_USER} -d $${POSTGRES_DB}"]
      interval: 10s                         # проверяем командой самой бд выше каждые 10 секунд
      timeout: 5s                           # ожидаем ответа 5 секунд
      retries: 5                            # всего пытаемся 5 раз
    volumes:                                # тут указываются подмаппленная часть диска, он смаппится с хост машины в сервис-бд
      - db-data:/var/lib/postgresql/data
	  
  search:                                   # 4. Именуем сервис с генератором поиска
    image: docker.elastic.co/elasticsearch/elasticsearch:7.17.22
    environment:
      discovery.type: single-node           # * это встроенная переменная elastic
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:9200/_cluster/health"]
      interval: 10s
      timeout: 5s
      retries: 5
    volumes:
      - es-data:/usr/share/elasticsearch/data

volumes:                                    # все тома (volumes) надо потом тут указывать
  db-data:
  es-data:
```
* `docker compose up --build` - из папки с `docker-compose.yaml` собираем+запускаем нашу пачку докерфайлов
* `docker compose up` - запустить уже собранный пак
# network
> По умолчанию docker compose, например, создаёт сеть и шарит её на все контейнеры внутри. Можно не делать для нескольких контейнеров compose и просто создать такую же сеть вручную.

* `docker network create <desired name>`, сети всё так же есть разных типов `bridge, host, none ...`
* `docker network ls` - список сетей
# swarm
> Built-in оркестратор, немного проще k8s, как в механизмах, так и в освоении.