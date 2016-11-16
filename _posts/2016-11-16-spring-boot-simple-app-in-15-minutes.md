---
title: "Spring Boot приложение за 15 минут"
excerpt: "В статье будет представлен простейший пример Spring Boot приложения и особенности его конфигурации"
modified: 2016-11-16T09:55:10-04:00
header:
tags: 
  - java
  - spring boot
  - thymeleaf
---

## Что вы сделаете?
Вы сделаете простейшее Spring Boot приложение. В качестве view будет использоваться технология _thymeleaf_. 
Вместо контроллеров мы будем использовать "проброску" сразу на view с помощью [_ViewControllerRegistry_](http://docs.spring.io/spring-framework/docs/3.2.5.RELEASE/javadoc-api/org/springframework/web/servlet/config/annotation/ViewControllerRegistry.html)

## Что вам понадобится?
 * Около 15-50 минут
 * Ваш любимый текстовый редактор или IDE
 * JDK 1.8 или выше
 * Maven 3.0+

## Как пользоваться этой статьей
 * Внимательно прочитайте статью
 * Скачайте исходный код с github
 * Запустите и проверьте работу
 * Удостоверьтесь что код вам понятен
 * Если что то осталось неясно, смело задавайте вопросы мне или _google_
 
## Thymeleaf
Давайте создадим gui-часть или view для нашего приложения. Так как приложение простое, то
нам понадобятся всего лишь 2 странички выполненные по технологии [_Thymeleaf_](https://en.wikipedia.org/wiki/Thymeleaf)

Thymeleaf это Java XML/XHTML/HTML5 движок шаблонов(template engine) который может работать как в web(с помощью сервлетов) 
так и не в web окружениях(например для форматирования emails).
Лучше всего конечно он подходит для слоя view в MVC-приложениях и имеет хорошую интеграцию со Spring.

Считается, что Thymeleaf это замена JSP. Шаблоны Thymeleaf можно открывать прямо в браузере как обычные html странички.
[Thymeleaf и какие у него есть преимущества перед JSP/JSTL?](https://toster.ru/q/346456)
{: .notice}

Вот пример _base.html_ странички:

```html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml" xmlns:th="http://www.thymeleaf.org" xmlns:sec="http://www.thymeleaf.org/thymeleaf-extras-springsecurity3">
    <head>
        <title>Spring Boot Simple Example</title>
    </head>
    <body>
        <h1>Hello %User name%!</h1>
        <p>Please click <a th:href="@{/hello}">here</a> to see a message.</p>
    </body>
</html>
```

## Spring MVC
Наше приложение базируется на Spring MVC. Поэтому нам надо сконфигурировать наши контроллеры для
того, чтобы наши Thymeleaf шаблоны показались пользователю. Однако в этот раз нам не понадобится
создавать отдельные классы контроллеров. Мы воспользуемся такой удобной функциональностью из
библиотеки Spring как [_ViewControllerRegistry_](http://docs.spring.io/spring-framework/docs/3.2.5.RELEASE/javadoc-api/org/springframework/web/servlet/config/annotation/ViewControllerRegistry.html)

```java
@Configuration
public class MvcConfig extends WebMvcConfigurerAdapter {
    
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/base").setViewName("base");
        registry.addViewController("/").setViewName("base");
        registry.addViewController("/hello").setViewName("hello");
    }

}
```
## Spring Boot
Насмотря на то, что название статьи указывается как Spring Boot, часть про него получилась самая короткая.
Однако насмотря на свою краткость аннотация _@SpringBootApplication_ скрывает под собой следующее:

 * @Configuration - помечает класс как источник бинов для контекста приложения(application context) 
 * @EnableAutoConfiguration - указывает Spring Boot начать добавлять бины базируясь на настройках classpath, других бинах, и других настроечных параметрах.
 * @ComponentScan - указывает Spring искать компоненты, конфигурации и сервисы в пакете данного класса.
 * @EnableWebMvc - обычно вы добавляете эту аннотацию в ваше Spring MVC приложение, но Spring Boot добавляет ее автоматически когда 
   видит _spring-webmvc_ в classpath
 
Обратите внимание, что _spring-webmvc.jar_ добавился в наш проект после того, как мы подключили так называемый _spring boot starter_
Подробнее тут: [getting-started-first-application-dependencies](http://docs.spring.io/spring-boot/docs/current/reference/html/getting-started-first-application.html#getting-started-first-application-dependencies)
{: .notice}

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

Для того, чтобы запустить наше приложение нужно просто выполнить обычный main метод который содержит в себе _SpringApplication.run()_.

Заметьте, что вам не пришлось ни устанавливать _Tomcat_ ни править _web.xml_. Веб-приложение 100% написано на java
и вам не нужно конфигурировать сотни xml и настраивать инфраструктуру бессонными ночами.
{: .notice}

```java
@SpringBootApplication
public class Application {

    public static void main(String[] args) throws Throwable {
        SpringApplication.run(Application.class, args);
    }

}
```

## Запуск
 * Запустите main-метод
 * Откройте в браузере [http://localhost:8080](http://localhost:8080)

## Код приложения
Код на github можно скачать по следующей ссылке: TODO

По мотивам: [https://spring.io/guides/gs/securing-web](https://spring.io/guides/gs/securing-web)
