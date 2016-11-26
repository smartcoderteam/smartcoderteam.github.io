---
title: "Как очень быстро удалить файлы из каталога в Windows7?"
excerpt: "Некоторые программы создают файлы с очень длинными именами или очень много файлов, например _node_modules_"
modified: 2016-11-26T09:55:10-04:00
header:
tags: 
  - win7
  - node_modules
  - useful
---

## Проблема

Удаляя как то ненужные проекты основанные на _node.js_ столкнулся с тем, что стандартными средствами windows удалить 
каталог _node_modules_ или ОЧЕНЬ долго или невозможно в принципе. Так как файловая система не поддерживает длинные имена
в unix-подобном стиле.

## Решение 1

Одним из решением может быть специальная
утилита предназначенная именно для этих целей [_rimraf_](http://www.nikola-breznjak.com/blog/javascript/nodejs/how-to-delete-node_modules-folder-on-windows-machine/):

```
npm install rimraf -g
rimraf node_modules
```

## Решение 2

Однако если у вас Windows то вы можете воспользоваться командой _robocopy_
Следующая команда позволит очистить проблемный каталог в РАЗЫ быстрее по сравнению с вариантом ручного удаления через _FAR_ или _TotalCommander_:

```
robocopy /MIR c:\test D:\UserData\FolderToDelete > NUL
```

Другие способы читайте тут: [unable-to-delete-node-modules-folder-windows-7](http://stackoverflow.com/questions/28175200/unable-to-delete-node-modules-folder-windows-7)




