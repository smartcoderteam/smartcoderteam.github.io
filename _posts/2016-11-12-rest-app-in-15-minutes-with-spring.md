---
title: "REST-приложение за 15 минут с помощью Spring"
excerpt: "В статье будет представлен простейший пример REST-приложения с использованием Spring"
modified: 2016-11-12T09:55:10-04:00
header:
tags: 
  - java
  - spring framework
  - rest
---

## Введение
REST (Representational State Transfer — «передача состояния представления») - архитектурный стиль
взаимодействия компонентов в распределенном приложении. Это не технология которую вы можете купить или
библиотека которую вы сможете установить в свой проект. Это набор принципов которые сформулировал
Рой Филдинг (англ. Roy Fielding) в своей диссертации. Он один из создателей протокола «HTTP». 
Эта работа она скорее академична, хотя не так сложна для понимания. 

Вообщем можно охарактеризовать подход как набор архитектурных ограничений налагая которые на разрабатываемую 
систему мы можем добиться определенных преимуществ. Этот архитектурный стиль и набор ограничений
не предполагает бездумное использование его где только возможно.

Считается что Всемирная Паутина(Web) является одной из возможных реализаций обсуждаемого архитектурного стиля.
Давайте перечислим кратко эти ограничения:

 * Клиент-серверная архитектура
 * Отсутствие состояния
 * Кэширование
 * Единообразие интерфейса
   - Манипуляция ресурсами через представление. 
   - Идентификация ресурсов
   - «Самоописываемые» сообщения
   - Гипермедиа, как средство изменения состояния приложения
 * Взаимодействие через слои

Для веб-служб, построенных с учётом REST (то есть не нарушающих накладываемых им ограничений), применяют термин «RESTful».

Подробнее читайте тут:
 
 * [wiki about REST](https://ru.wikipedia.org/wiki/REST)
 * [rest-foundations-restful](https://dzone.com/refcardz/rest-foundations-restful)
 * [fielding/pubs/dissertation](http://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm)

## Пример приложения

Мы с вами разработаем сервис под названием _myplay_ как аналог магазина google play. Весьма амбициозно, не так ли? :)
Он позволит вернуть клиенту набор программ разбитых по подкатегориям.

В данной статье хотелось бы показать практичный пример REST-подобного приложения. Чтобы уложиться в заявленный 15-минутный формат
придется ограничиться лишь одним критерием. Однако надо заметить что он является наиболее характерным и всегда упоминаемым когда говорят про _RESTful services_.

 * Идентификация ресурсов:
  Все REST сервисы обычно организованы в набор ресурсов. Ресурс это набор однотипных данных: 
  пользователи, товары в интернет магазине, категории, программы и т.п. Каждый ресурс идентифицируется одним или больше
  URI(Uniform Resource Identifiers). Для того чтобы получить доступ к ресурсу приложение должно использовать одну из всеми известных 
  HTTP-операций(GET, POST, PUT, DELETE, etc). 
  
 * https://{service}.yourservice.com/{version}/{resourcepath}

 * примеры URI которые мы будем использовать в нашем приложении:
   1 https://apps.myplay.com/v1/categories 
   2 https://apps.myplay.com/v1/categories/1
   3 https://apps.myplay.com/v1/categories/1/apps
   4 https://apps.myplay.com/v1/categories/2/apps/3
 
Применяя HTTP-глагол GET на URI _1_ можно будет получить список доступных категорий для наших программ.
POST же создаст нам новую категорию. DELETE удалит все категории. И так далее. Логика довольно проста.
Использовать то, что уже прекрасно работает и широко применяется. 

Важно понимать, что применяя GET запрос мы получаем не сам ресурс, а лишь его _представление_ например в JSON/XML.
Первая буква в REST как раз говорит об этом - R(Representational State - состояние представления).
Таким образом, получая лишь представления о состоянии ресурса клиент может каким то образом взаимодействовать с системой.
Существует возможность указать серверу какое представление нам подходит через задание content-type:

```
curl –u user:password -d @categories.xml -H "Content-type: text/xml" http://myplay.com/categories
```

Подробнее почерпнуть знания про создание URI для вашего сервиса можно тут:
 
 * [rest_api_guide/uri-general](https://developer.yahoo.com/social/rest_api_guide/uri-general.html)

## Реализация

### Конфигурация Spring

Конфигурация приложения описана в классе _AppConfig_. 

```java
@Configuration
@EnableWebMvc
@ComponentScan(basePackages = "ru.smartcoder.spring_rest_example")
```

 * @Configuration сообщает Spring, что это конфигурационные настройки нашего приложения
 * @EnableWebMvc импортирует Spring MVC конфигурацию из WebMvcConfigurationSupport
 * @ComponentScan делает доступным для IOC контейнера spring все наши: контроллеры и компоненты аннотированные @Component и    @RestController

Отдельно хотелось бы упомянуть про класс _AppInitializer_.
Начиная с выпуска спецификации Servlet 3.0 стало возможным сконфигурировать сервлет-контейнер почти без xml.
Совершенно точно, нам не нужен никакой _web.xml_. Класс _ServletContainerInitializer_ позволяет нам зарегестрировать
нам все те артефакты, что мы определяли раньше в _web.xml(filters, listeners, servlets etc)_

Подробнее можно почитать тут: 
[when-use-abstractannotationconfigdispatcherservletinitializer-and-webapplication](http://stackoverflow.com/questions/26676782/when-use-abstractannotationconfigdispatcherservletinitializer-and-webapplication)

### Хранилище данных

Создадим классы _AppCategoriesDao_ и _ApplicationsDao_ которые будут возвращать данные. 
Для простоты это будут _in-memory_ хранилище.

```java
@Component
public class AppCategoriesDao {

    private static List<AppCategory> categories;

    {
        categories = new ArrayList<>();
        categories.add(new AppCategory(1L, "Games"));
        categories.add(new AppCategory(2L, "Multimedia"));
        categories.add(new AppCategory(3L, "Productivity"));
        categories.add(new AppCategory(4L, "Tools"));
        categories.add(new AppCategory(5L, "Health"));
        categories.add(new AppCategory(6L, "Lifestyle"));
    }
    ...
```

### REST-endpoints via Spring MVC Controllers

Контроллеры _ApplicationController_ и _CategoriesController_ будут содержать следующие аннотации:
 * @RestController
 * @GetMapping
 * @DeleteMapping
 * @PutMapping
 * @PostMapping
 
```java
@RestController
public class ApplicationController {

    private static final String VERSION = VERSION_1_URL;

    @Autowired
    private ApplicationsDao applicationsDao;

    @GetMapping("/" + VERSION + "/" + CATEGORIES_URL + "/{categoryId}/" + APPLICATIONS_URL)
    public List<Application> getApplicationsByCategory(@PathVariable Long categoryId) {
        return applicationsDao.list(categoryId);
    }
    ...
```

### JSON

Также подключим в наш _pom.xml_ зависимость от чудевсной библиотеки jackson, которая позволит автоматически преобразовывать
java-объекты в JSON.
 
```xml
 <dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>${jackson.version}</version>
 </dependency>
``` 

### Локальный tomcat

Добавление следующего плагина в _pom.xml_ позволит нам запускать приложение из локального tomcat:

```xml
<plugin>
    <groupId>org.apache.tomcat.maven</groupId>
    <artifactId>tomcat7-maven-plugin</artifactId>
    <version>2.2</version>
    <configuration>
        <port>9090</port>
        <path>/</path>
    </configuration>
</plugin>
```
Используйте maven-команду: _tomcat7:run_ 

### Результат

Открыв бразузер по следующему адресу:
http://localhost:9090/v1/categories
Мы получим следующий JSON:
```
[{
	"id": 1,
	"name": "Games"
},
{
	"id": 2,
	"name": "Multimedia"
},
{
	"id": 3,
	"name": "Productivity"
},
{
	"id": 4,
	"name": "Tools"
},
{
	"id": 5,
	"name": "Health"
},
{
	"id": 6,
	"name": "Lifestyle"
}]
```

Попробуйте скачать код по ссылке и поиграться сами:

TODO





 
