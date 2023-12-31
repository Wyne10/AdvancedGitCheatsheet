# Advanced Git Cheatsheet (WIP)

Этот список содержит все git команды, которые зачастую не показывают в гайдах для новичков, однако я нашел их полезными. Я не стал включать команды которые я посчитал не нужными для себя, поэтому если вы не нашли нужной для себя команды, советую посетить страницу команды в документации git.

В основном этот список содержит только **ПРИМЕРЫ** команд, это значит, что в большинстве случаев вы можете комбинировать команды, ключевые слова так, как хотите.

Заголовки параграфов являются гиперссылками на страницу команды на сайте [документации git.](https://git-scm.com/docs)

Основную информацию брал с сайта https://www.atlassian.com/git/tutorials и https://git-scm.com/docs/

Большинство команд я не проверял на практике, поэтому в случае ошибок в объяснении или команде, буду рад правкам.
## [Init](https://git-scm.com/docs/git-init)
- `git init --bare` - Создать "bare" репозиторий, в котором нельзя напрямую делать коммиты.
- `git init <directory> --template=<template_directory>` - Создать репозиторий по шаблону.
---
## [Clone](https://git-scm.com/docs/git-clone)
- `git clone -depth=1 <repo>` - Клонировать репозиторий с определенной глубиной коммитов.
- `git clone --branch <branch>`  - Позволяет клонировать определенную ветку.
- `git clone --origin <name>` - Переименовывает ссылку на удаленный репозиторий.
---
## [Config](https://git-scm.com/docs/git-config)
- `git config --global alias.<alias> <command>` - Создание алиасов.
- `git config --list` - Вывести список всех настроек.
- `git config --global init.defaultBranch <name>` - Сменить название дефолтной ветки.
---
## [Add](https://git-scm.com/docs/git-add)
- `git add --patch` - Позволяет по частям выбрать какие изменения добавить.
---
## [Commit](https://git-scm.com/docs/git-commit)
- `git commit --patch` - Позволяет по частям выбрать какие изменения применить. 
- `git commit --amend` - Вместо создания нового коммита, добавит изменения в прошлый коммит.
- `--fixup` - Помогает исправить прошлый коммит, без создания нового коммита. `--fixup` помечает все коммиты которые нужно будет объединить с исправляемым коммитом, `--autosquash` автоматически объединит все коммиты с исходными коммитами на которые они ссылались. `--fixup` не изменит сообщение исправляемого коммита.
```git
git commit --fixup=<commit>
git rebase --autosquash HEAD~<n>
```

Кроме этого --fixup обладает комбинированными командами:
- `git commit --fixup=amend:<commit>` - Делает тоже самое, что и обычный `--fixup`, но также предоставляет возможность изменить сообщение исправляемого коммита.
- `git commit --fixup=reword:<commit>` - Создаст `--fixup`, однако изменит только сообщение исправляемого коммита, не трогая его содержимое.
- `git commit --squash=<commit>` - Объединит последний коммит и выбранный коммит, для окончательного применения нужно снова написать `git commit`, но без `--squash`. Применяется если нужно объединить более мелкие коммиты в один большой. (Зачастую squash удобнее производит через `git rebase -i`)
- `git commit --reuse-message=<commit>` - Использовать комментарий, включая информацию о авторе и время другого коммита.
- `git commit --reedit-message=<commit>` - Использовать комментарий другого коммита, но с возможностью редактирования.
- `git commit --dry-run` - Не создавать коммит, но вывести информацию.
- `--reset-author` - При использовании с `--reuse-message`, `--reedit-message`, `--amend` сменит автора и время коммита.
- `--author=<author>` - Переписать автора.
- `--date=<date>` - Переписать дату.
- Каноничный формат комментария коммита
```git
Change the message displayed by hello.py

- Update the sayHello() function to output the user's name - Change the sayGoodbye() function to a friendlier message
```
---
## Dot notation
- **.** - Одинарная точка используется как сокращение ссылки на текущую директорию на текущей ветке.
- **..** - Двойная точка имеет разный смысл в зависимости от контекста, обычно она используется для представления диапазона или сравнений.
- **...** - Тройная точка представляет симметричную разницу, например с помощью команды `git log <branch1>...<branch2>` можно увидеть коммиты уникальные для каждой ветки и не разделяющиеся между ними.

Далее будут приведены примеры с точками, для лучшего понимания.

---
## [Diff](https://git-scm.com/docs/git-diff)

Сюда я напихал много всяческих примеров, для того чтобы у вас появилось понимание, что команды git очень гибкие и имеют много применений, многие задачи могут быть решены разными способами.

- `git diff` - Показать изменения того что изменено, но не кэшировано.
- `git diff --cached` - Показать разницу того что кэшировано.
- `git diff --git a/<file> b/<file>` - Показать разницу текущего файла и последнего коммита этого файла. (`a/` - текущая директория, `b/` - прошлый коммит)
- `git diff --color-words` - Режим с цветовым выделение разницы.
- `git diff HEAD..` - Показать разницу последнего коммита и текущей рабочей директории.
- `git diff <commit/branch> <commit2/branch2>` - Сравнить два разных коммита/ветки.
- `git diff <commit/branch> <commit2/branch2> <file>` - Сравнить один файл на двух разных коммитах/ветках. 
- `git diff <commit>..<commit2>` - Показать разницу между коммитами `<commit>` (исключительно) до `<commit2>` (включительно). (Здесь используется ".." как диапазон от `<commit>` до `commit2`)
- `git diff <commit/branch>...<commit2/branch2>` - Покажет разницу между двумя коммитами/ветками, но исключит файлы которые существуют на обоих коммитах/ветках.
---
## [Stash](https://git-scm.com/docs/git-stash)
- `git stash` - Сохранить изменение вне коммита на будущее использование.
- `git stash save <description>` - Сохранить добавив описание.
- `git stash --patch` - Сохранить файлы частично.
- `--include-untracked` - Включить в сохранение неотслеживаемые файлы.
- `-all` - Включить в сохранение все файлы, включая игнорируемые.
- `git stash list` - Показать список сохранений.
- `git stash pop` - Применить последнее сохранение и удалить его из стеша.
- `git stash pop stash@{<n>}` - Применить выбранное сохранение и удалить его из стеша.
- `git stash apply` - Применить последнее сохранение, но не удалять его из стеша.
- `git stash apply stash@{<n>}` - Применить выбранное сохранение, но не удалять его из стеша.
- `git stash show` - Показывает краткое содержание сохранения.
- `git stash show --patch` - Позволяет показать полный Diff сохранения.
- `git stash branch <newbranch> stash@{<n>}` - Создает ветку из сохранения.
- `git stash drop stash@{<n>}` - Удаляет сохранение.
- `git stash clear` - Удаляет все сохранения.
---
## [.gitignore](https://git-scm.com/docs/gitignore)

- .gitignore паттерны

|Паттерн|Пример совпадения|Объяснение|
|---|---|---|
|`**/logs`|`logs/debug.log`  <br>`logs/monday/foo.bar`  <br>`build/logs/debug.log`|Двойная звезда сопоставляет директорий где угодно в репозитории.|
|`**/logs/debug.log`|`logs/debug.log`  <br>`build/logs/debug.log`  <br>_but not_  <br>`logs/build/debug.log`|Также двойная звезда сопоставляет файлы на основе их имени и имени родителького каталога.|
|`*.log`|`debug.log`  <br>`foo.log`  <br>`.log`  <br>`logs/debug.log`|Звезда сопоставляет ноль или более символов.|
|`*.log`  <br>`!important.log`|`debug.log`  <br>`trace.log`  <br>_but not_  <br>`important.log`  <br>`logs/important.log`|Добавление восклицательного знака к паттерну отменяет его. Если файл соответствует паттерну, но также соответствует паттерну отрицания, определенному позже в файле, он не будет проигнорирован.|
|`*.log`  <br>`!important/*.log`  <br>`trace.*`|`debug.log`  <br>`important/trace.log`  <br>_but not_  <br>`important/debug.log`|Паттерны определённые после отменяющего паттерна будут повторно игнорировать все ранее отмененные файлы.|
|`/debug.log`|`debug.log`  <br>_but not_  <br>`logs/debug.log`|Добавление слеша сопоставляет только файлы в корне репозитория.|
|`debug.log`|`debug.log`  <br>`logs/debug.log`|По умолчанию паттерны сопоставляют файлы в любой директории.|
|`debug?.log`|`debug0.log`  <br>`debugg.log`  <br>_but not_  <br>`debug10.log`|Знак вопроса сопоставляет ровно один знак.|
|`debug[0-9].log`|`debug0.log`  <br>`debug1.log`  <br>_but not_  <br>`debug10.log`|Квадратный скобки могут также быть использованы для сопоставления одного символа в указанном диапазоне.|
|`debug[01].log`|`debug0.log`  <br>`debug1.log`  <br>_but not_  <br>`debug2.log`  <br>`debug01.log`|Квадратные скобки сопоставляют единичный символ из указанного списка.|
|`debug[!01].log`|`debug2.log`  <br>_but not_  <br>`debug0.log`  <br>`debug1.log`  <br>`debug01.log`|Восклицательный знак может быть использован для сопоставления любого символа кроме символа из указанного списка.|
|`debug[a-z].log`|`debuga.log`  <br>`debugb.log`  <br>_but not_  <br>`debug1.log`|Диапазоны могут быть числовые или буквенные.|
|`logs`|`logs`  <br>`logs/debug.log`  <br>`logs/latest/foo.bar`  <br>`build/logs`  <br>`build/logs/debug.log`|Если не добавлять слеш, паттерн будет сопоставлять и файлы и содержимое директорий с этим именем, в примере слева и директории и файлы с именем _logs_ проигнорированы.|
|`logs/`|`logs/debug.log`  <br>`logs/latest/foo.bar`  <br>`build/logs/foo.bar`  <br>`build/logs/latest/debug.log`|Добавление слеша указывает, что паттерн это директория. Всё содержимое любого каталога в репозитории с подходящим именем, включая все содержимые файлы и поддиректории будут проигнорированы.|
|`logs/`  <br>`!logs/important.log`|`logs/debug.log`  <br>`logs/important.log`|Подождите минутку! Разве `logs/important.log` не должен быть отменен на примере слева?  <br>  <br>Нет! Из-за причуд связанных с производительностью в Git, вы не можете отменить файл который проигнорирован из-за паттерна сопоставляющего директорию.|
|`logs/**/debug.log`|`logs/debug.log`  <br>`logs/monday/debug.log`  <br>`logs/monday/pm/debug.log`|Двойная звездочка сопоставляет ноль или более директорий.|
|`logs/*day/debug.log`|`logs/monday/debug.log`  <br>`logs/tuesday/debug.log`  <br>_but not_  <br>`logs/latest/debug.log`|Одиночная звезда также может быть использована в названиях директорий.|
|`logs/debug.log`|`logs/debug.log`  <br>_but not_  <br>`debug.log`  <br>`build/logs/debug.log`|Паттерны, определяющие файл в определенном каталоге, относятся к корню репозитория. (Вы можете добавить слеш, но это не делает ничего особенного.)|

- С помощью знака `#` можно добавлять комментарии.
- С помощью знака `\` можно избегать символы используемые в паттернах.
- Можно создать глобальные правила игнорирования
```git
touch ~/.gitignore
git config --global core.excludesFile ~/.gitignore
```
- Если нужно проигнорировать файл, который уже был в коммите ранее, можно использовать команду `git rm --cached <file>`, сам файл удалён не будет.
- `git add --force <file>` - С помощью этой команды можно добавить проигнорированный файл в коммит.
- `git check-ignore --verbose <file>` - С помощью этой команды можно увидеть почему игнорируется тот или иной файл.
  Вывод команды:
  `<файл содержащий паттерн> : <номер строки паттерна> : <паттерн>    <имя файла>`
---
## [Status](https://git-scm.com/docs/git-status)
- `--short` - Вывод в коротком формате.
- `--show-stash` - Показать текущее количество вхождений сохраненных в стеше.
- `--ignored` - Показать игнорируемые файлы и директории.
- `git status --verbose` - Кроме названия измененного файла, также покажет изменения внутри файла, если указать `--verbose` (`-v`) дважды, то будут также показаны изменения файлов которые еще не были добавлены через `git add`.
---
## [Log](https://git-scm.com/docs/git-log)
- `git log --oneline` - Вывод в коротком формате.
- `git log --stat` - Кроме обычной информации, также покажет изменённые файлы и количество изменённых строк.
- `git log --max-count=<number>` - Вывести последние `<number>` коммитов.
- `git log --patch` - Выведет патч каждого коммита, это покажет полный diff каждого коммита.
- `git log --author=pattern>` - Выведет коммиты определённого автора, аргумент может быть использован как обычная строка или regex.
- `git log --grep=<pattern>` - Выведет коммиты с определённым сообщением, аргумент может быть использован как обычная строка или regex.
- `git log <file>` - Отобразить только коммиты которые включаёт в себя определённый файл.
- `git log --skip=<number>` - Пропустить `<number>` коммитов перед началом вывода коммитов.
- `git log <since>..<until>` - Отобразить только коммиты произошедшие между `<since>` и `<until>`, аргументы могут быть ID коммитов, именами веток или любыми другими ссылками на ревизию.
- `git log -S"<text>" --pretty=format:'%h %an %ad %s'` - Отобразит все произошедшие изменения определённого текста.
---
## [Tag](https://git-scm.com/docs/git-tag)
- `git tag <tagname>` - Создать новый тег, замените `<tagname>` семантическим идентификатором состояния репозитория на момент создания тега. Распространенным шаблоном является использование номеров версий, таких как `git tag v1.4`.
- `git tag --annotate <tagname>` - Создать аннотированный тег, пример выше был облегчённым тегом, который содержит только название тега, аннотированные теги также содержат имя тегера, электронную почту, дату и т.д.
- `git tag --annotate <tagname> -m "<comment>"` - Эта команда создаст аннотированный тег с указанным комментарием без необходимости открывать редактор текста.
- `git tag` - Выведет список всех тегов.
- `git tag --list '<wildcard>'` - Выведет только теги совпадающие с `<wildcard>`.
- `git tag --annotate <tagname> <commitID>` - Создаст тег для выбранного коммита.
- `git tag --force <tagname>` - Заменит содержимое уже существующего тега.
- `git tag --delete <tagname>` - Удалить тег.

Для отправки тегов на удалённый репозиторий можно использовать команды 
`git push <remote> <tagname>` или `git push <remote> --tags`.

Для просмотра состояния репозитория в теге, можно использовать команду `git checkout <tagname>`.
___
## [Blame](https://git-scm.com/docs/git-blame)
- `git blame <file>` - Выведет информацию про изменения, их авторов и даты.
- `git blame -L <start>,<end> <file>` - Ограничит вывод от `<start>` до `<end>` строк.
- `git blame --show-email <file>` - Отобразить email вместо имени автора.
- `git blame -w <file>` - Игнорирует изменённые пробелы.
- `git blame -M <file>` - Обнаружит скопированные или перемещённые линии внутри файла и выведет их оригинального автора.
- `git blame -C <file>` - Обнаружит скопированные или перемещённые линии вне файла и выведет их оригинального автора.
