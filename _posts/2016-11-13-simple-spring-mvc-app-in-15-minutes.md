---
title: "Spring MVC приложение за 15 минут"
excerpt: "В статье будет представлен простейший пример Spring MVC приложения и особенности его конфигурации"
modified: 2016-11-13T09:55:10-04:00
header:
tags: 
  - java
  - spring framework
  - mvc
---

## Введение
Spring web фреймворк разработан, чтобы облегчить жизнь современному enterprise разработчику создающему web-приложения.
Он базируется на популярном архитектурном шаблоне: MVC(Model-View-Controller). Приложения получается
слабо связными и гибкими в настройке как и сам Spring фреймворк.

## Жизненный цикл запроса
Каждый раз когда пользователь нажимает ссылку или отправляет форму в web-браузере запрос отправляется на сервер.
Давайте посмотрим на схему обработки запроса в Spring MVC(ссылка кликабельна и ведет на документацию на spring.io):

[![image-left]({{ site.url }}{{ site.baseurl }}/images/spring-mvc-request-processing.jpg)](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/mvc.html)

Spring MVC использует подход называемый [wiki/Front_controller](https://en.wikipedia.org/wiki/Front_controller).
В роли главного сервлета или front controller,  принимающего все входящие запросы выступает _DispatcherServlet_. 
Работа DispatcherServlet состоит в том, чтобы послать запрос на соответствующий Spring MVC контроллер.
Он пользуется помощью так называемых _handler mappings_ которые помогают ему определить какой именно
контроллер нужно вызвать.

После этого контроллер начинает отрабатывать. 

Стоит заметить, что обычно хорошо спроектированный контроллер 
делегирует работу по выполнению запроса на какой либо сервисный метод содержащий бизнес-логику.
{: .notice}

Обычно после выполнения бизнес-логики пользователю нужно показать какую то информацю как результат.
Эта информация обычно храниться в модели. Однако информация пеед показом должна пройти обработку, чтобы
принять удобоваримый вид, например HTML. Для этого контроллер посылает модель и название view обратно
в _DispatcherServlet_. Таким образом контролеер отвязан от конкретной реализации view. 
_DispatcherServlet_ посылает запрос в _view resolver_ чтобы понять какую действительно HTML-страницу нужно показать
пользователю.

## Конфигурация

Начиная с версии Servlet 3.0 мы можем использовать не _web.xml_ а Java-конфигурацию.
Рекомендуется наследоваться от _AbstractAnnotationConfigDispatcherServletInitializer_ который 
предоставляет возможность настроить 2 контекста: _AppConfig_ и _WebConfig_

```java
public class WebAppInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {

    @Override
    protected Class<?>[] getRootConfigClasses() {
        // AppConfig defines beans that would be in root-context.xml
        return new Class[] { AppConfig.class };
    }

    @Override
    protected Class<?>[] getServletConfigClasses() {
        // WebConfig defines beans that would be in servlet.xml
        return new Class[] { WebConfig.class };
    }

    @Override
    protected String[] getServletMappings() {
        return new String[] { "/" };
    }

}
```

Важно понимать разницу между корневым _ApplicationContext_ и _WebServletApplicationContext_.
Первый обычно содержит все сервисные и инфрастуктурные бины вашего приложения.
Второй обычно содержит контекст относящийся к отдельному _DispatcherServlet_. 

Как вы уже догадались, в отдельном приложении может быть несколько _DispatcherServlet_.
{: .notice}

Таким образом, отдельные бины могут быть переопределены в _WebServletApplicationContext_, если это нужно.

Рекомендую почитать информацию по этой теме тут: [what-is-the-difference-between-applicationcontext-and-webapplicationcontext](http://stackoverflow.com/questions/11708967/what-is-the-difference-between-applicationcontext-and-webapplicationcontext-in-s)

[![image-left]({{ site.url }}{{ site.baseurl }}/images/mvc-context-hierarchy.png)](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/mvc.html)

В простейшем случае настройка _WebAppContext_ сведется к такому Java-config:

```java
@Configuration
@EnableWebMvc
@ComponentScan(basePackages = "ru.smartcoder.spring_mvc_example.controller")
public class WebConfig extends WebMvcConfigurerAdapter {

    @Bean
    public ViewResolver viewResolver() {
        InternalResourceViewResolver viewResolver = new InternalResourceViewResolver();
        viewResolver.setViewClass(JstlView.class);
        viewResolver.setPrefix("/WEB-INF/views/");
        viewResolver.setSuffix(".jsp");

        return viewResolver;
    }

    @Override
    public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
        configurer.enable();
    }

}
```

 * @EnableWebMvc поможет включить Spring MVC без дополнительных xml настроек
 * _viewResolever_ метод создаст бин типа _ViewResolver_ который поможет _DispatcherServlet_ определить нужную JSP для отображения.
 * метод _configureDefaultServletHandling_ перенаправит все запросы на статические ресурсы с _DispatcherServlet_
   на сервлет вашего контейнера для лучшей производительности [what-does-configuredefaultservlethandling-means](http://stackoverflow.com/questions/29396281/what-does-configuredefaultservlethandling-means)
 
 
Таким может быть настройка корневого контекста:

```
@Configuration
@ComponentScan(basePackages={"ru.smartcoder.spring_mvc_example.service"},
        excludeFilters={
                @ComponentScan.Filter(type= FilterType.ANNOTATION, value=EnableWebMvc.class)
        })
public class AppConfig {
}
```

Заметьте, что для того, чтобы исключить из _@ComponentScan_ конфигурацию с аннотацией _@EnableWebMvc_
мы использовали параметр _excludeFilters_. В данном приложении это необязательно, однако стоит иметь ввиду имеющиеся 
возможности у component scan [preventing-a-enablewebmvc-annotated-class](http://stackoverflow.com/questions/21527578/preventing-a-enablewebmvc-annotated-class-from-being-picked-up-by-componentsca)

## Запуск
Для того, чтобы запустить приложение выполните следующую команду maven: 

```tomcat7:run```

Зайдя по ссылке [http://localhost:9090/hello](http://localhost:9090/hello) вы увидите что наш контроллер отработал успешно.

## Заключение
Рабочий код можно скачать по ссылке: [https://github.com/levrun/spring-mvc-example](https://github.com/levrun/spring-mvc-example)






