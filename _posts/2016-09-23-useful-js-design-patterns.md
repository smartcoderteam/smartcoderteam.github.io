---
title: "Полезные шаблоны проектирования в JavaScript"
excerpt: "В этом посте мы познакомимся с некоторыми шаблонами проектирования в JavaScript."
modified: 2016-09-09T09:55:10-04:00
header:
tags: 
  - patterns
  - JavaScript
---

## В этом посте мы познакомимся со следующими шаблонами проектирования в JavaScript:

  * [Функции как абстракции](#functions_as_abstractions)
  * [Функции для создания модулей](#functions_as_modules)
  * [Немедленно вызываемые функциональные выражения (IIFE)](#functions_as_iife)
  
### <a name="functions_as_abstractions"></a>Функции как абстракции
Функция может быть передана как указатель и обернута в вызов:

```javascript
var work = function()
{
    console.log("working hard");
}
 
var doWork = function(f)
{
    console.log("starting");
 
    try
    {
        f();
    }
    catch(ex)
    {
        console.log(ex);
    }
 
    console.log("ending");
}
 
doWork(work);
```

### <a name="functions_as_modules"> Функции для создания модулей
Функции могут быть использованы как объекты, которые имеют члены, методы и приватные переменные:

```javascript
var worker = createWorker();
 
worker.job1();
worker.job2();
```

Этого можно добиться так:

```javascript
var createWorker = function()
{
    return
    {
        job1: function()
        {
            console.log("task1");
        },
 
        job2: function()
        {
            console.log("task2");
        }
 
    };
 
}
```

Или лучше так:

```javascript
var createWorker = function()
{
    // Private variable
    var workCount = 0;
 
    var task1 = function()
    {
        workCount+=1;
        console.log("task1" + workCount);
    };
 
    var task2 = function()
    {
        workCount+=1;
        console.log("task2" + workCount);
    };
 
    return
    {
        job1: task1,
        job2: task2
    };
 
};
 
var worker = createWorker();
```

### <a name="functions_as_iife"></a> Немедленно вызываемые функциональные выражения (IIFE)

Один из недостатков вышеприведенных шаблонов – это то, что мы создаем глобальные переменные. 
К примеру, createWorker – это [глобальная](http://en.wikipedia.org/wiki/Global_variable) переменная.
createWorker создает такие scopes и переменные, как workCount, task1 и task2, которые видны только внутри createWorker.
В Javascript глобальные переменные – это зло. 
Очень легко переопределить глобальную переменную, объявленную кем-то еще. 
Как же нам избежать этого? Один способ – это обернуть createWorker внутрь другой функции:


```javascript
var program = function()
{
    var createWorker = function()
    {
        // Private variable
        var workCount = 0;

        var task1 = function()
        {
            workCount+=1;
            console.log("task1" + workCount);
        };

        var task2 = function()
        {
            workCount+=1;
            console.log("task2" + workCount);
        };

        return
        {
            job1: task1,
            job2: task2
        };

    };

    var worker = createWorker();
    worker.job1();
    worker.job2();

}

program();
```

Однако, тут мы по-прежнему имеем глобальную переменную program. Как мы можем избавиться и от нее?
Введя такое понятие, как [IIFE](http://en.wikipedia.org/wiki/Immediately-invoked_function_expression) – immediately-invoked function expression (немедленно вызываемое функциональное выражение):

```javascript
(function()
{
    var createWorker = function()
    {
        // Private variable
        var workCount = 0;

        var task1 = function()
        {
            workCount+=1;
            console.log("task1" + workCount);
        };

        var task2 = function()
        {
            workCount+=1;
            console.log("task2" + workCount);
        };

        return
        {
            job1: task1,
            job2: task2
        };

    };

    var worker = createWorker();
    worker.job1();
    worker.job2();

}());
```

Теперь все переменные определены в IIFE.
Этот шаблон используется в большом количестве JavaScript-библиотек – например, таких как JQuery.
По мотивам [www.madhur.co.in](http://www.madhur.co.in/blog/2014/12/15/javascript-design-patterns.html)
