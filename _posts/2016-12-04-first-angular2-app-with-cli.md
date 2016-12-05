---
title: "Ваше первое Angular2 приложение с помощью CLI"
excerpt: "В статье будет представлен пример создания Angular2-приложения с помощью CLI"
modified: 2016-12-04T09:55:10-04:00
header:
tags: 
  - angular2
  - node js
  - type script
---

## Что вы сделаете?
Простейшее приложнеие с помощью [Angular CLI](https://en.wikipedia.org/wiki/Command-line_interface).

## Что вам понадобится?
 * Около 15-50 минут
 * Ваш любимый текстовый редактор
 * NodeJS
 * NPM(Node Package Manager)

## Как пользоваться этой статьей
 * Внимательно прочитайте статью
 * Выполните все инструкции
 * Проверьте что приложение работает 
 * Попробуйте создать отдельное приложение сами без инструкций
 * Если что то осталось неясно, смело задавайте вопросы мне или _google_
 
## Установите Node Package Manager

Он нам понадобиться, чтобы установить [Angular CLI](https://cli.angular.io/). Это добавления в интерфейс командной строки или
попросту консоль которые позволят вам _быстро создать рабочую заготовку_ для вашего нового Angular2 проекта тем самым экономя ваше время.

Перейдите на сайт [nodejs.org](https://nodejs.org) и скачайте последнюю версию NodeJS. Установив NodeJS вы также получите и NPM(NodePackageManger).
Это именно то, что нам нужно для установки Angular CLI.

## Установите Angular CLI

 * Выполните следующую команду:
 
 ```
 npm install -g angular-cli@latest
 ```
 
 Пользователям Windows рекомендуется запускать консоль из под администратора!
{: .notice}

## Создание и запуск проекта 

 * Выполните следующий набор команд:
 
 ```
 ng new first-app
 ng serve
 ```
 
 * ng new - создаст заготовку вашего приложения, причем вполне рабочую и готовую к запуску 
 * ng serve - запустит локальный web-сервер

```
$ ng new first-app
installing ng2
  create .editorconfig
  create README.md
  create src\app\app.component.css
  create src\app\app.component.html
  ...
  create tslint.json
Successfully initialized git.
Installing packages for tooling via npm.
Installed packages for tooling via npm.
```

```
$ ng serve
** NG Live Development Server19678ms building modules                                                       4ms add02761ms 206ms asset49ms emitting
Hash: 90558ee085669621107f
Version: webpack 2.1.0-beta.25
Time: 23559ms
            Asset       Size  Chunks             Chunk Names
   main.bundle.js    2.72 MB    0, 2  [emitted]  main
 styles.bundle.js    10.3 kB    1, 2  [emitted]  styles
 inline.bundle.js    5.54 kB       2  [emitted]  inline
  main.bundle.map    2.82 MB    0, 2  [emitted]  main
styles.bundle.map    14.1 kB    1, 2  [emitted]  styles
inline.bundle.map     5.6 kB       2  [emitted]  inline
       index.html  482 bytes          [emitted]
Child html-webpack-plugin for "index.html":
```

Вам осталось лишь открыть браузер и ввести следующую строку [http://localhost:4200/](http://localhost:4200/)

```
app works!
```

## Давайте попробуем поменять сообщение

Зайдите в каталог _first-app/src/app/app.components.ts_ и поменяйте значение переменной _title_

```
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'app works! update';
}
```

_ng serve_ автоматически "увидит" изменения в файле и пересоберет проект. Посмотрите изменения в браузере:

```
app works! update
```

## Структура проекта

Давайте кратко опишем ключевые файлы в нашем созданном проекте, чтобы вы имели общее представление:

```
  src\app\app.component.css <--- стили для отдельного компонента
  src\app\app.component.html <--- html-шаблон или view для компонента
  src\app\app.component.spec.ts <--- тесты 
  src\app\app.component.ts <--- компонент
  src\app\app.module.ts <--- настройки модуля
  src\environments\environment.prod.ts
  src\environments\environment.ts
  src\index.html <--- главный html файл который запускает приложение
  src\main.ts <--- точка входа в наше приложение
  src\polyfills.ts <--- поддержка полифилов для type script [dom-polyfill](https://learn.javascript.ru/dom-polyfill)
  src\styles.css <--- глобальные стили общие для всего приложения
  src\tsconfig.json <--- настройки type-script для приложения
  angular-cli.json <--- настройки CLI
  e2e\app.e2e-spec.ts <--- end to end тесты
  e2e\tsconfig.json <--- настройки type-script в e2e тестах
  karma.conf.js <--- нужно для unit-тестирования вашего будущего приложения
  package.json <--- NPM конфигурация
  protractor.conf.js <--- нужно для тестирования e2e вашего будущего приложения
  tslint.json <--- настройки code-style
```





