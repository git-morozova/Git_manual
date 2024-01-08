[⬅  к содержанию](../readme.md)
## 8. Другие полезные команды
В последней части этого руководства мы расскажем о некоторых дополнительных трюках, которые могут вам помочь.
&nbsp;<br>
&nbsp;<br>
### Отслеживание изменений, сделанных в коммитах
У каждого коммита есть свой уникальный идентификатор в виде строки цифр и букв. Чтобы **просмотреть список всех коммитов и их идентификаторов**, можно использовать команду `log`:
```
git log
commit ba25c0ff30e1b2f0259157b42b9f8f5d174d80d7
Author: Tutorialzine
Date: Mon May 30 17:15:28 2016 +0300
New feature complete
commit b10cc1238e355c02a044ef9f9860811ff605c9b4
Author: Tutorialzine
Date: Mon May 30 16:30:04 2016 +0300
Added content to hello.txt
commit 09bd8cc171d7084e78e4d118a2346b7487dca059
Author: Tutorialzine
Date: Sat May 28 17:52:14 2016 +0300
Initial commit
```

Как вы можете заметить, идентификаторы довольно длинные, но для работы с ними не обязательно копировать их целиком — первых нескольких символов будет вполне достаточно. Чтобы посмотреть, **что нового появилось в коммите**, мы можем воспользоваться командой `show [commit]`
```
git show b10cc123
commit b10cc1238e355c02a044ef9f9860811ff605c9b4
Author: Tutorialzine
Date: Mon May 30 16:30:04 2016 +0300
Added content to hello.txt
diff --git a/hello.txt b/hello.txt
index e69de29..b546a21 100644
--- a/hello.txt
+++ b/hello.txt
@@ -0,0 +1 @@
+Nice weather today, isn't it?
```

Чтобы увидеть **разницу между двумя коммитами**, используется команда `diff` (с указанием промежутка между коммитами):
```
git diff 09bd8cc..ba25c0ff
diff --git a/feature.txt b/feature.txt
new file mode 100644
index 0000000..e69de29
diff --git a/hello.txt b/hello.txt
index e69de29..b546a21 100644
--- a/hello.txt
+++ b/hello.txt
@@ -0,0 +1 @@
+Nice weather today, isn't it?
```

Мы сравнили первый коммит с последним, чтобы увидеть все изменения, которые были когда-либо сделаны. Обычно проще использовать `git difftool`, так как эта команда запускает графический клиент, в котором наглядно сопоставляет все изменения.
&nbsp;<br>
&nbsp;<br>

### Возвращение файла к предыдущему состоянию
Git позволяет **вернуть выбранный файл к состоянию на момент определенного коммита**. Это делается уже знакомой нам командой `checkout`, которую мы ранее использовали для переключения между ветками. Но она также может быть использована для переключения между коммитами (это довольно распространенная ситуация для Git - использование одной команды для различных, на первый взгляд, слабо связанных задач).

В следующем примере мы возьмем файл *hello.txt* и откатим все изменения, совершенные над ним к первому коммиту. Чтобы сделать это, мы подставим в команду идентификатор нужного коммита, а также путь до файла:
```
git checkout 09bd8cc1 hello.txt
```
&nbsp;<br>
&nbsp;<br>

### Исправление коммита
Если вы опечатались в комментарии или забыли добавить файл и заметили это сразу после того, как закоммитили изменения, вы легко можете это поправить при помощи `commit —amend`. Эта команда добавит все из последнего коммита в область подготовленных файлов и попытается сделать новый коммит. Это дает вам возможность **поправить комментарий или добавить недостающие файлы в область подготовленных файлов**.

Для более сложных исправлений, например, не в последнем коммите или если вы успели отправить изменения на сервер, нужно использовать `revert`. Эта команда создаст коммит, отменяющий изменения, совершенные в коммите с заданным идентификатором.

Самый последний коммит может быть доступен по алиасу HEAD:
```
git revert HEAD
```

Для остальных будем использовать идентификаторы:
```
git revert b10cc123
```

При отмене старых коммитов нужно быть готовым к тому, что возникнут **конфликты**. Такое случается, если файл был изменен еще одним, более новым коммитом. И теперь Git не может найти строчки, состояние которых нужно откатить, так как они больше не существуют.
&nbsp;<br>
&nbsp;<br>

### Разрешение конфликтов при слиянии
Помимо сценария, описанного в предыдущем пункте, конфликты регулярно возникают при слиянии ветвей или при отправке чужого кода. Иногда конфликты исправляются автоматически, но обычно с этим приходится разбираться вручную — решать, какой код остается, а какой нужно удалить.

Давайте посмотрим на примеры, где мы попытаемся слить две ветки под названием *john_branch* и *tim_branch*. И Тим, и Джон правят один и тот же файл: функцию, которая отображает элементы массива.

Джон использует цикл:
```
// Use a for loop to console.log contents.
for(var i=0; i<arr.length; i++) {
console.log(arr[i]);
}
```

Тим предпочитает `forEach`:
```
// Use forEach to console.log contents.
arr.forEach(function(item) {
console.log(item);
});
```

Они оба коммитят свой код в соответствующую ветку. Теперь, если они попытаются слить две ветки, они получат сообщение об ошибке:
```
git merge tim_branch
Auto-merging print_array.js
CONFLICT (content): Merge conflict in print_array.js
Automatic merge failed; fix conflicts and then commit the result.
```

Система не смогла разрешить конфликт автоматически, значит, это придется сделать разработчикам. **Приложение отметило строки, содержащие конфликт**:
```
<<<<<<< HEAD // Use a for loop to console.log contents. for(var i=0; i<arr.length; i++) { console.log(arr[i]); } ======= // Use forEach to console.log contents. arr.forEach(function(item) { console.log(item); }); >>>>>>> Tim's commit.
```

Над разделителем ======= мы видим последний (HEAD) коммит, а под ним - конфликтующий. Таким образом, мы можем увидеть, чем они отличаются и **решать, какая версия лучше. Или вовсе написать новую**. В этой ситуации мы так и поступим, перепишем все, удалив разделители, и дадим Git понять, что закончили.
```
// Not using for loop or forEach.
// Use Array.toString() to console.log contents.
console.log(arr.toString());
```
Когда все готово, нужно закоммитить изменения, чтобы закончить процесс:
git add -A
git commit -m "Array printing conflict resolved."
```

Как вы можете заметить, процесс довольно утомительный и может быть очень сложным в больших проектах. Многие разработчики предпочитают использовать для разрешения конфликтов клиенты с графическим интерфейсом. (Для запуска нужно набрать `git mergetool`).
&nbsp;<br>
&nbsp;<br>

### Настройка .gitignore
В большинстве проектов есть файлы или целые директории, в которые мы не хотим (и, скорее всего, не захотим) коммитить. Мы можем удостовериться, что они случайно не попадут в `git add -A` при помощи файла *.gitignore*

1. Создайте вручную файл под названием *.gitignore* и сохраните его в директорию проекта.

2. Внутри файла перечислите названия файлов/папок, которые нужно игнорировать, каждый с новой строки.

3. Файл *.gitignore* должен быть добавлен, закоммичен и отправлен на сервер, как любой другой файл в проекте.

Вот хорошие примеры файлов, которые нужно игнорировать:

- Логи
- Артефакты систем сборки
- Папки *node_modules* в проектах *node.js*
- Папки, созданные IDE, например, *Netbeans* или *IntelliJ*
- Разнообразные заметки разработчика.

Файл *.gitignore*, исключающий все перечисленное выше, будет выглядеть так:
```
*.log
build/
node_modules/
.idea/
my_notes.txt
```

Символ слэша в конце некоторых линий означает директорию (и тот факт, что мы рекурсивно игнорируем все ее содержимое). Звездочка, как обычно, означает шаблон.

---
&nbsp;<br>
[далее  ➡](commands.md)