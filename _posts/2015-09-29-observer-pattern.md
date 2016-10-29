---
title: "Паттерн Observer"
excerpt: "Паттерн Observer – это поведенческий шаблон проектирования. Он позволяет извещать набор объектов, так называемых Listeners (или Subscribers), об изменении состояния у какого-то другого объекта..."
modified: 2015-09-29T09:55:10-04:00
author: "Anton Mironyuk"
header:
tags: 
  - design patterns
  - java
---

<link href="http://alexgorbatchev.com/pub/sh/current/styles/shThemeDefault.css" rel="stylesheet" type="text/css" />
<script src="http://alexgorbatchev.com/pub/sh/current/scripts/shCore.js" type="text/javascript"></script>
<script src="http://alexgorbatchev.com/pub/sh/current/scripts/shAutoloader.js" type="text/javascript"></script>

<script type="text/syntaxhighlighter" class="brush: js"><![CDATA[
  function foo()
  {
      if (counter <= 10)
          return;
      // it works!
  }
]]></script>

### Что это такое
Паттерн Observer – это [поведенческий шаблон проектирования](https://ru.wikipedia.org/wiki/%D0%9F%D0%BE%D0%B2%D0%B5%D0%B4%D0%B5%D0%BD%D1%87%D0%B5%D1%81%D0%BA%D0%B8%D0%B5_%D1%88%D0%B0%D0%B1%D0%BB%D0%BE%D0%BD%D1%8B_%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D1%8F). 
Он позволяет извещать набор объектов, так называемых Listeners (или Subscribers), 
об изменении состояния у какого-то другого объекта Subject (или Publisher). 
Если брать аналогию из жизни, то можно представить себе следующую картинку – подписка на газеты.
Читатели подписываются на выход свежего номера газеты. Издатель извещает подписчиков при выходе свежего номера. 
В данной модели издатель один, а подписчиков (читателей) много. Таким образом, получаем связь один-ко-многим.

![image-left]({{ site.url }}{{ site.baseurl }}/images/publisher_suscribers.jpg)

### Реализация
Давайте рассмотрим диаграмму классов данного шаблона проектирования. 
Где Subject – сущность издателя, изменения в которой транслируются всем наблюдателям. 
Observer – сущность наблюдателя (слушателя), реализованная таким образом, что позволяет принимать сообщения от Subject.

![image-left]({{ site.url }}{{ site.baseurl }}/images/observer_pattern_uml_diagram.jpg)

### Шаг 1
Реализуем класс Subject:

```
import java.util.ArrayList;
import java.util.List;

public class Subject {
	
   private List<Observer> observers = new ArrayList<Observer>();
   private int state;

   public int getState() {
      return state;
   }

   public void setState(int state) {
      this.state = state;
      notifyAllObservers();
   }

   public void attach(Observer observer){
      observers.add(observer);		
   }

   public void notifyAllObservers(){
      for (Observer observer : observers) {
         observer.update();
      }
   } 	
}
```

### Шаг 2
Создадим абстрактный класс Observer:

```
public abstract class Observer {
   protected Subject subject;
   public abstract void update();
}
```

### Шаг 3
Создадим конкретные классы наших наблюдателей, наследуясь от Observer:

```
public class BinaryObserver extends Observer{

   public BinaryObserver(Subject subject){
      this.subject = subject;
      this.subject.attach(this);
   }

   @Override
   public void update() {
      System.out.println( "Binary String: " + Integer.toBinaryString( subject.getState() ) ); 
   }
}
```

```
public class OctalObserver extends Observer{

   public OctalObserver(Subject subject){
      this.subject = subject;
      this.subject.attach(this);
   }

   @Override
   public void update() {
     System.out.println( "Octal String: " + Integer.toOctalString( subject.getState() ) ); 
   }
}
```


```
public class HexaObserver extends Observer{

   public HexaObserver(Subject subject){
      this.subject = subject;
      this.subject.attach(this);
   }

   @Override
   public void update() {
      System.out.println( "Hex String: " + Integer.toHexString( subject.getState() ).toUpperCase() ); 
   }
}
```

### Шаг 4
Используем созданные выше классы:

```
public class ObserverPatternDemo {
   public static void main(String[] args) {
      Subject subject = new Subject();

      new HexaObserver(subject);
      new OctalObserver(subject);
      new BinaryObserver(subject);

      System.out.println("First state change: 15");	
      subject.setState(15);
      System.out.println("Second state change: 10");	
      subject.setState(10);
   }
}
```

### Результат
Заметьте, как с помощью метода attach() мы присоединяем наблюдателя к нашему издателю. 
Теперь при вызове метода setState() все наблюдатели будут извещены:

```
First state change: 15
Hex String: F
Octal String: 17
Binary String: 1111
Second state change: 10
Hex String: A
Octal String: 12
Binary String: 1010
```

По мотивам Design Patterns – [Observer Pattern](http://www.tutorialspoint.com/design_pattern/observer_pattern.htm)

