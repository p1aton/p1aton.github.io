---
layout: post
title:  "Git шпаргалка"
crawlertitle: "This is our first post"
summary: "Основные команды git"
date:   2020-06-19 23:09:47 +0700
categories: posts
tags: ['Дата аналитика']
#author: Felipe
---



Команды Git - это важный урок, который в какой-то момент должен усвоить каждый аналитик данных. Чтобы использовать весь потенциал популярной системы управления версиями Git, необходимо знать, как использовать популярные Git команды.

#### Часто используемые команды Git

### Git Setup

{% highlight python %}
git init [directory]   --- Превращает текущий каталог в репозиторий Git
git clone [repo / URL]   --- клонировать / загрузить репозиторий на локальный компьютер
git clone [URL] [folder]   --- клонировать репозиторий из удаленного места в указанную папку | [папка] | на вашем локальном компьютере
{% endhighlight %}

### Git Configuring

{% highlight python %}
git config --global user.name "[your_name]"   --- установить имя автора, которое будет прикреплен ко всем коммитам текущего пользователя
git config --global user.email "[email_address]" --- установить адрес электронной почты, который будет прикреплен ко всем коммитам текущего пользователя
git config --globalcolor.ui auto  --- установить автоматическую командную строку Git раскраска
git config --global alias.[alias_name][git_command]    --- создать ярлык (псевдоним) для Git команд
git config --systemcore.editor[text_editor]    --- установить текстовый редактор по умолчанию для всех пользователей на машине
git config --global --edit   ---открыть глобальную конфигурацию Git файла
{% endhighlight %}

###  Managing Files

{% highlight python %}
git status   --- показывает состояния файлов в рабочей директории и индексе: какие файлы изменены, но не добавлены в индекс; какие ожидают коммита в индексе.
git log    --- используется для просмотра истории коммитов, начиная с самого свежего и уходя к истокам проекта. 
git log --all     --- используется для просмотра истории коммитов из всех веток
git log start-branch..end-branch    --- перечисляет все коммиты, доступные из end-branch , которые недоступны из start-branch
git log start-branch...end-branch    --- перечисляет все коммиты, доступные либо из start-branch , либо из end-branch , но недоступные как из start-branch , так и из end-branch 
git dif   --- увидеть разницу между рабочим каталогом и индексом (каких изменений не было еще совершено )
get diff --cached    --- увидеть разницу между последним коммитом и индексом
get diff HEAD     --- увидеть разницу между последним коммитом и рабочим каталогом
git show [object]     --- используется для просмотра информации о метке или коммите 
{% endhighlight %}

### Git Branches

{% highlight python %}
git branch   --- умеет перечислять ваши ветки, создавать новые, удалять и переименовывать их
git branch -a     --- перечисляет все удаленные ветки
git branch [branch]     --- умеет составлять список ваших веток, создавать новые, удалять и переименовывать их
git checkout [branch]   --- переключиться на другую ветку (либо существующий или создать новую ветку под указанным именем]
git branch -d [branch]    --- удалить локальную ветку
git branch -m [new_branch_name]   --- переименовывает ветку, в которой вы находитесь в настоящее время
git merge [branch]    --- объединият указанную ветку с текущей веткой
{% endhighlight %}

### Rewriting History

{% highlight python %}
git commit --amend     --- изменяет предыдущий коммит, вместо того чтобы создавать новый
git rebase [base]     --- переносит текущую ветку поверх <base>. <base> - может быть хешем коммита, именем ветки, тегом или относительной ссылкой на HEAD.
git reflog    --- перечисляет изменения, внесенные в HEAD локального репозитория
{% endhighlight %}

### Remote Repositories

{% highlight python %}
git remote add [name] [URL]    --- создает новое соединение с удаленным репозиторием и дайет ему имя, чтобы служить ярлыком для URL
git fetch [remote_repo] [branch]    --- получает ветку с удаленного хранилища
git pull [remote_repo]     --- получить указанный репозиторий и объединить его с локальной копией
git push [remote_repo] [branch]    --- отправляет ветку в удаленный репозиторий со всеми ее коммитами и объектами
{% endhighlight %}

### Undoing Changes

{% highlight python %}
git revert [file/directory]    --- отменяет все изменения в указанном файле / каталоге, создает новый коммит и применяет его к текущей ветке
git reset [file]    --- отключает указанный файл без перезаписи и изменений
git reset [commit]   --- отменить все изменения, которые произошли после конкретного коммита
git clean -n    --- посмотреть, какие файлы следует удалить из текущего каталога
git clean -f    --- удалит ненужные файлы в каталоге
{% endhighlight %}

### Making Changes

{% highlight python %}
git add [file/directory]     --- добавляет содержимое рабочей директории в индекс (staging area) для последующего коммита
git add .    --- работает только в текущей директории
git commit -m "[descriptive_message]"   --- записать изменения с заданным сообщением
{% endhighlight %}



### Ubuntu: Show your branch name on your terminal

Add these lines in your ~/.bashrc file

{% highlight python %}
# Show git branch name
force_color_prompt=yes
color_prompt=yes
parse_git_branch() {
 git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
}
if [ "$color_prompt" = yes ]; then
 PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[01;31m\]$(parse_git_branch)\[\033[00m\]\$ '
else
 PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w$(parse_git_branch)\$ '
fi
unset color_prompt force_color_prompt
{% endhighlight %}



<!---

sudo apt-get install git

git --version

Вы увидите сообщение:

{% highlight python %}
git version 2.29.2.windows.2
{% endhighlight %}

{% highlight python %}
git config --global user.name "Analitik Datascientistov"
# вводите своё имя или ник латиницей и в кавычках
{% endhighlight %}


{% highlight python %}
git config --global user.email analitikds@yandex.com
# здесь нужно ввести свой реальный e-mail
{% endhighlight %}

роверьте, что получилось: выполните команду git config с опцией --list (англ. list,
«список»):

{% highlight python %}
git config --list
# вывели в окно командной строки список всех свойств конфига
{% endhighlight %}

--->

<!---

ssh

git remote -v
git remote set-url origin git@github.com:p1aton/p1aton.github.io.git
git remote -v
git push

git branch -a посмотреть все ветки
git checkout next-js переключится на другую ветку (пример next-js )
--->

<!---
git branch - смотрим ветку
git checkout -b images - создаем новую ветку и заходим в нее 
git branch -a посмотреть все ветки

git remote -v - команда показывает имя удаленного репо, если такой имеется в наличии. Ключ –v показывает путь к удаленному репо.
git pull upstream master - проверить и скачать изменения с удаленного репозитория
git remote add upstream(имя) ссылка на репо - добавить удаленный репозиторий

git diff master - показывает разницу
git diff ...master - из мастер к нам
git diff master... - из другого репо к нам

git commit --amend --no-edit - внести изменения в последний комит (добовляемся к текущему коммиту)

git pull address(url gitrepo) master - обновляемся с основного репо
git push - отпровляем в свой форк


Лучше да. Но можно и не обязательно. Разницу все равно будет смотреть с твоей стороны в мою, а не наоборот. И если конфликтов не будет, то все итак норм залетит.
А вообще, на будущее, лучше делать так:
1. Перед началом выполнения задачи переключаешься в мастер и делает пулл с основного проекта.
2. Делаешь новую ветку.
3. Коммитишь и пушишь в гитхаб.
4. Отправляешь ПР
5. После принятия ПР локально дропаешь эту ветку и забываешь про нее

Теперь можешь локально дропнуть бранч git branch -D branchName



Андрей
а если ветку мастер сначала сохранил и закоммитил? он мне в github пишет что различий нет

Перейти на мастер
git checkout master
Стереть последний коммит
git reset HEAD~
Ну и заменить что есть на мастере
git push -f origin master




    общий сценарий заключается в следующем: я забыл создать новую ветку для новой функции, и делает всю работу в старую ветку. Я поручил всю" старую "работу главной ветви, и я хочу, чтобы моя новая ветвь выросла из"Мастера". Я не сделал ни одного заявления о своей новой работе. Вот отраслевая структура: "мастер"->"Old_feature"

    git stash 
    git checkout master
    git checkout -b "New_branch"
    git stash apply

    https://pingvinus.ru/git/1718


--->


