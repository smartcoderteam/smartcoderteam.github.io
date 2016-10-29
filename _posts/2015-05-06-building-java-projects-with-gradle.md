---
title: "Сборка Java-проекта с помощью Gradle"
excerpt: "В этом посте мы создадим простейшее приложение на Java и соберем его с помощью системы сборки Gradle."
modified: 2015-05-06T09:55:10-04:00
header:
tags: 
  - build
  - gradle
---

В этом посте мы создадим простейшее приложение на Java и соберем его с помощью системы сборки Gradle. Для этого от вас потребуется:

 * Около 15 минут времени
 * Любимый текстовый редактор
 * JDK версии 6+
{: .notice}

  * [Настройка проекта](#project_setup)
  * [Установка Gradle](#gradle_setup)
  * [Что может Gradle](#what_gradle_can)
  * [Сборка Java-проекта](#gradle_build)
  * [Работа с зависимостями](#work_with_dependencies)


### <a name="project_setup"></a>Настройка проекта
Сперва создадим простейшее приложение на Java, которое нам нужно будет собрать. 
Создайте следующую структуру каталогов в вашей папке с проектами:

```terminal
simple_java_project_with_gradle/src/main/java/hello
```
И поместите туда два файла (_HelloWorld.java_ и _Greeter.java_):

```java
package hello;

public class HelloWorld {
  public static void main(String[] args) {
    Greeter greeter = new Greeter();
    System.out.println(greeter.sayHello());
  }
}
```

```java
package hello;

public class Greeter {
  public String sayHello() {
    return "Hello world!";
  }
}
```

### <a name="gradle_setup"></a>Установка Gradle
Теперь, когда у нас есть простейший Java-проект, который мы хотим собрать, настало время установить Gradle. 
Gradle можно скачать в виде zip-архива [тут](http://www.gradle.org/downloads). 
Нам нужны только бинарники, так что ищите ссылку на скачивание вида _gradle-version-bin.zip_. 
Также вы можете скачать _gradle-version-all.zip_, чтобы получить исходники и документацию в дополнение к бинарникам.

Распакуйте файл и добавьте путь к каталогу /bin в cистемную переменную PATH. 
Для того, чтобы протестировать установку, запустите из командной строки следующую команду:

```terminal
gradle
```

Если все пойдет хорошо, вы увидите приветственное сообщение:

```terminal
:help

Welcome to Gradle 2.0.
To run a build, run gradle <task> ...
To see a list of available tasks, run gradle tasks
To see a list of command-line options, run gradle --help
BUILD SUCCESSFUL

Total time: 2.675 secs
You now have Gradle installed.
```

Это значит, что Gradle успешно установился на вашем компьютере.

### <a name="what_gradle_can"></a>Что может Gradle
Теперь, когда Gradle установлен, посмотрим, что он может. Для этого вам нужно спросить у Gradle, какие задачи вам доступны:

```terminal
gradle tasks
```

Выполнив эту команду, вы сможете увидеть список доступных задач. 
Если вы запустите эту команду не из каталога, где есть _build.gradle_ файл, скорее всего, вы увидите примерно такой список:

```terminal
:tasks

------------------------------------------------------------
All tasks runnable from root project
------------------------------------------------------------

Build Setup tasks
-----------------
init - Initializes a new Gradle build. [incubating]
wrapper - Generates Gradle wrapper files. [incubating]

Help tasks
----------
dependencies - Displays all dependencies declared in root project 'user'.
dependencyInsight - Displays the insight into a specific dependency in root proj
ect 'user'.
help - Displays a help message
projects - Displays the sub-projects of root project 'user'.
properties - Displays the properties of root project 'user'.
tasks - Displays the tasks runnable from root project 'user'.

To see all tasks and more detail, run with --all.

BUILD SUCCESSFUL

Total time: 2.718 secs
```

### <a name="gradle_build"></a>Сборка Java-проекта
Создадим простейший текстовый файл _build.gradle_, который поместим в корень нашего проекта. 
Добавим туда только лишь одну строку:

```terminal
apply plugin: 'java'
```

Эта единственная строка в нашей конфигурации сборки проекта привносит огромное количество возможностей. 
Запустите команду ‘gradle tasks‘ снова и вы увидите новые задачи, добавленные в список, 
включая задачи для сборки проекта, создания Javadoc и выполнения unit-тестов.

Часто вы будете использовать задачу ‘gradle build‘. 
Эта задача компилирует программный код, запускает unit-тесты и собирает результаты компиляции в JAR-файлы. 
Вы можете запустить ее следующим образом:

```terminal
gradle build
```

Через несколько секунд вы увидите сообщение “BUILD SUCCESSFUL”, подтверждающее, что сборка была завершена успешно. 
Для того, чтобы увидеть результаты сборки, загляните в каталог /build – вы увидите там несколько директорий, включая следующие:

 * _/classes_ – содержит cкомпилированные .class файлы проекта (включая наши HelloWorld.class и Greeter.class);
 * _/reports_ – содержит отчеты, созданные в результате сборки (например отчеты по выполнению unit-тестов);
 * _/libs_ – содержит создаваемые проектные библиотеки (обычно JAR- и/или WAR-файлы);
 * _/dependency_cache_ – содержит зависимости, необходимые для сборки (на данный момент проект не содержит никаких зависимостей и поэтому здесь ничего нет).

### <a name="work_with_dependencies"></a>Работа с зависимостями
Наше простейшее приложение ‘Hello world!‘ полностью независимо от сторонних и дополнительных библиотек. 
Однако большинство приложений часто имеют зависимости от внешних библиотек при использовании уже готовой функциональности.

Для примера представим, что вы хотите, чтобы наше приложение в дополнение к строчке “Hello World!” 
печатало текущую дату и время. 
Для этих целей вы, конечно, можете использовать возможности JDK, но давайте используем возможности библиотеки Joda Time.

Сперва измените файл HelloWorld.java следующим образом:

```java
package hello;

import org.joda.time.LocalTime;

public class HelloWorld {
  public static void main(String[] args) {
    LocalTime currentTime = new LocalTime();
    System.out.println("The current local time is: " + currentTime);

    Greeter greeter = new Greeter();
    System.out.println(greeter.sayHello());
  }
}
```

Здесь в классе HelloWorld используется класс _LocalTime_ из библиотеки _Joda Time_ для получения текущего времени.
Если вы сейчас запустите сборку с помощью задачи ‘gradle build‘, то увидите, что она завершится ошибкой. 
Это произойдет из-за того, что вы не объявили библиотеку Joda Time как компилируемую зависимость в файле _build.gradle_.
Чтобы исправить это, вам нужно сперва добавить в _build.gradle_ источник для сторонних библиотек (так называемых 3rd party libraries):

```terminal
repositories {
    mavenCentral()
}
```

Блок repositories показывает, что при сборке все зависимости должны быть взяты из репозитория Maven Central. 
Gradle очень много взял от системы сборки Maven, включая возможность использовать Maven Central как источник зависимостей.

Теперь, когда мы готовы к использованию сторонних библиотек, давайте их объявим:

```terminal
dependencies {
    compile "joda-time:joda-time:2.2"
}
```

В блоке dependencies вы объявляете единственную зависимость от Joda Time. 
Если говорить точнее, вы указываете (читая справа налево), что вам нужна версия 2.2 библиотеки joda-time в группе joda-time.

Еще стоит заметить, что данная зависимость имеет тип compile. 
Это означает, что она будет доступна во время компиляции (если вы будете собирать WAR-файл, она будет включена в соответствующий каталог _/WEB-INF/libs_).

Другие возможные типы зависимостей:

 *  _providedCompile_ – зависимость требуется для компиляции, но предполагается, что в дальнейшем она будет поставляться контейнером, исполняющим код;
 * _testCompile_ – зависимость используется для компиляции и выполнения тестов, но не требуется на этапе выполнения программы.

Наконец, давайте зададим блок jar:

```terminal
jar {
    baseName = 'gs-gradle'
    version =  '0.1.0'
}
```

Блок jar определяет, как будет называться итоговый JAR-файл. В нашем случае сгенерится _gs-gradle-0.1.0.jar_.
Если вы сейчас запустите сборку с помощью команды ‘gradle build‘, Gradle должен использовать зависимость Joda Time из репозитория Maven Central. 
В результате, сборка пройдет без ошибок. 

Поздравляю вас – вы попробовали систему сборки Gradle в деле.
Чтобы погрузиться в тему более обстоятельно, вы можете почитать [официальное руководство](http://gradle.org/docs/current/userguide/userguide).

По мотивам: [Building Java Projects with Gradle](http://spring.io/guides/gs/gradle/)
