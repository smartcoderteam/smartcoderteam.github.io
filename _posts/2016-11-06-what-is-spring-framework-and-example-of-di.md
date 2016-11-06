---
title: "Что такое Spring Framework и для чего он нужен?"
excerpt: "В статье будет представлен простейший пример использования dependency injection в Spring"
modified: 2016-11-06T09:55:10-04:00
header:
tags: 
  - java
  - spring framework
  - dependency injection
---

## Введение
Spring-фреймворк обеспечивает комплексную программную и конфигурационную модель для 
современных Java-enterprise приложений.
Ключевой элемент Spring это инфраструктурная поддержка приложений таким образом,
чтобы команда разработчиков могла сфокусироваться на бизнес логике приложения которое 
они пишут, а не на технических деталях платформы на которую они устанавливают приложение.

## Возможности
 * [Dependency Injection](https://en.wikipedia.org/wiki/Dependency_injection)
 * Аспектно-ориентированное программирование включая декларативное управление транзакциями
 * Spring MVC и RESTful модули для создания веб-приложений
 * Поддержка JDBC, JPA, JMS
 * И много другого...

Все доступные фичи и модули описаны по ссылке [overview-modules](http://docs.spring.io/spring-framework/docs/current/spring-framework-reference/html/overview.html#overview-modules)

## Минимальные требования
 * JDK 6+ для Spring Framework 4.x
 * JDK 5+ для Spring Framework 3.x

## Простейший пример dependency injection

# Создадим maven проект с зависимостью от Spring
При этом подключим _spring-context_. Это один из основных модулей, который 
предоставляет нам механизмы dependency injection.

```
<dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>4.3.3.RELEASE</version>
        </dependency>
</dependencies>
```

# Создадим сущность _MessagePrinter_
Это класс который имеет зависимость от сервиса _MessageService_

```java
package hello;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class MessagePrinter {

    final private MessageService service;

    @Autowired
    public MessagePrinter(MessageService service) {
        this.service = service;
    }

    public void printMessage() {
        System.out.println(this.service.getMessage());
    }
}
```

# Создадим и сам сервис _MessageService_

```java
package hello;

public interface MessageService {
    String getMessage();
}
```

## Далее нам понадобиться магия Spring, чтобы связать их вместе

```java
package hello;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.*;

@Configuration
@ComponentScan
public class Application {

    @Bean
    MessageService mockMessageService() {
        return new MessageService() {
            public String getMessage() {
                return "Hello World! This is our dummy Message Service speaking.";
            }
        };
    }

    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(Application.class);
        MessagePrinter printer = context.getBean(MessagePrinter.class);
        printer.printMessage();
    }
}
```

Пример показывает концепцию Dependency Injection в действии, _MessagePrinter_ отвязан(decoupled) от реализации _MessageService_
при этом Spring Framework связывает эти классы вместе в конфигурации которая задается в _Application_ классе.

Вы можете скачать рабочий пример кода по следующей ссылке [spring-di-example](https://github.com/levrun/spring-di-example)

По мотивам: [spring-framework](http://projects.spring.io/spring-framework/)
