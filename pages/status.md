[⬅  к содержанию](../readme.md)

<style>
red { color: red }
yellow { color: yellow }
</style>
<red> red color markdown text</red>
<yellow> red color markdown text</yellow>


## 3. Как узнать статус репозитория

`status` — это еще одна важнейшая команда, которая показывает **информацию о текущем состоянии репозитория**: актуальна ли информация на нём, нет ли чего-то нового, что поменялось, и так далее. Запуск `git status` на нашем свежесозданном репозитории должен выдать:
```html
git status
On branch master
Initial commit
Untracked files:
(use <span style="color: #81b11e;">"git add ..."</span> to include in what will be committed)
hello.txt
<red> red color markdown text</red>
<yellow> red color markdown text</yellow>
```

Сообщение говорит о том, что файл *hello.txt* неотслеживаемый. Это значит, что файл новый и система еще не знает, нужно ли следить за изменениями в файле или его можно просто игнорировать. Для того, чтобы начать отслеживать новый файл, нужно его специальным образом объявить.

---
&nbsp;<br>
[далее  ➡](add.md)