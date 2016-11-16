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
Лучше всего конечно он подходит для слоя view в MVC-приложениях и имеет хорошую интеграцию с Spring.
gration.

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

## Spring Boot


## Код приложения
Код на github можно скачать по следующей ссылке: TODO

