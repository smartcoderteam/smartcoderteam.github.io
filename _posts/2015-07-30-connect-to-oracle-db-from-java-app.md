---
title: "Connect to Oracle DB from a Java application"
excerpt: "Рассмотрим подключение к Oracle используя JDBC"
modified: 2015-07-30T09:55:10-04:00
author: "Anton Mironyuk"
header:
tags: 
  - databases
  - java
---

### Как подключиться к БД Oracle из вашей Java-программы?
Очень просто – вам нужно использовать _JDBC_. Это API, созданный специально для платформы Java. 
JDBC расшифровывается как _Java DataBase Connectivity_. 
JDBC является набором классов и интерфейсов, содержащих методы для получения и обновления информации в БД. 
Под базой данных имеется в виду [реляционная БД](http://ru.wikipedia.org/wiki/%D0%A0%D0%B5%D0%BB%D1%8F%D1%86%D0%B8%D0%BE%D0%BD%D0%BD%D0%B0%D1%8F_%D0%B1%D0%B0%D0%B7%D0%B0_%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D1%85)

### Кратко про _JDBC_
В _JDBC_ для создания подключения используется класс-фабрика java.sql.DriverManager и класс _java.sql.Connection_:

```java
Connection connection = null;
        try {
            connection = DriverManager.getConnection(

```

Создав подключение, можно использовать объекты класса java.sql.Statement, 
которые служат для выполнения запросов к БД. Различают несколько видов Statement :

 * _Statement_ – общего назначения
 * _PreparedStatement_ – если нужен запрос с параметрами
 * _CallableStatement_ – если нужно выполнить хранимую процедуру

### Находим и подключаем Oracle JDBC драйвер
Перед написанием программы для доступа к Oracle нам нужно скачать драйвер [ojdbcxxx.jar](http://www.oracle.com/technetwork/database/features/jdbc/index-091264.html). 
После скачивания нужно будет добавить путь к нему в [Classpath](http://en.wikipedia.org/wiki/Classpath_%28Java%29). 
И только после того, как путь к драйверу содержится в _Classpath_, мы можем переходить к следующему шагу.

PS: До версии _JDBC 4.0 (Java SE6)_ для загрузки драйвера в JVM нужно было явно использовать метод Class.forName():
{: .notice}

```java
        try {
            Class.forName("oracle.jdbc.driver.OracleDriver");
        } catch (ClassNotFoundException e) {
            System.out.println("Oracle JDBC Driver not found");
            e.printStackTrace();
            return;
        }
```

Если вы используете _JDK_ версии 6+, то делать это необязательно.

### Пишем тестовую программу

```java
public class JdbcConnectionTest {

    public static void main(String[] args) {
        Connection connection = null;
        try {
            connection = DriverManager.getConnection("jdbc:oracle:thin:@oracle_server_url:1521/SID", "user", "password");
        } catch (SQLException e) {
            System.out.println("Connection error!");
            e.printStackTrace();
            return;
        }

        if (connection != null) {
            System.out.println("We succesfully connected to Oracle db!");
        } else {
            System.out.println("Connection error!");
        }
    }

}
```

Перед запуском программы убедитесь, что ваш Oracle-сервер работает и доступен с вашего компьютера и 
что вам известны следующие параметры, необходимые для подключения:

 * _SID_ – Oracle System ID
 * oracle_server_url
 * oracle_server_port
 * user
 * password

