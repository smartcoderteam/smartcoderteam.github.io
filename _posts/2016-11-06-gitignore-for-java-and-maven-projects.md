---
title: "gitignore для java и maven проектов"
excerpt: "В статье будет представлен простейший пример файла gitignore что позволит вам не сохранить по ошибке ненужную информацию в вашем репозитории"
modified: 2016-11-06T09:55:10-04:00
header:
tags: 
  - java
  - spring framework
  - dependency injection
---

Часто при создании небольших проектов или примеров необходимо чтобы git игнорировал
настройки различных IDEs(Idea, Eclipse) и систем сборки таких как _Maven_. 
На помощь нам приходит волшебный файл [gitignore](https://github.com/ntpl/ntpl/wiki/Git-Gitignore)

Здесь я приведу наиболее часто используемый мной файл _gitignore_ который позволит
случайно не закомитить ненужное. 

```java
# Eclipse
.classpath
.project
.settings/

# Intellij
.idea/
*.iml
*.iws

# Mac
.DS_Store

# Maven
log/
target/
```
Вам надо просто поместить этот файл с именем _.gitignore_ в корень вашего проекта и те папки которые
указаны в данных настройках уже не попадут к вам в репо.

Если вам все же нужен более изощренный _gitignore_ попробуйте использовать
автоматический генератор доступный по следующей ссылке: [https://www.gitignore.io/](https://www.gitignore.io/)
