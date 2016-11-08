# Zeptoboard (Зептоборда)
Распределенный анонимный форум. Форум является текстовым (ради безопасности 
и экономии места на диске и трафика), но считается приемлемым и безопасным
встраивание в сообщения ссылок на картинки с github cloud content, которые
можно просматривать либо копируя ссылку в новую адресную строку либо 
превращать их в картинки с помощью User script.

По сути своей форум является репозиторием, копия которого есть у каждого участника.

Для участия не нужно быть программистом, но хорошо бы понимать как работают
некоторые команды git.

Участники регулярно забирают друг у друга изменения и поддерживают в репозитории 
список ссылок на репозитории других участников.

Пулл-реквесты делаются друг другу исключительно с целью внести предложить ссылку на свой репозиторий.

Для репозиториев и пулл-реквестов используется этот сайт (github.com). 
Теоретически может использоваться любой другой сервер. Удобство github в онлайн-оформлении пулл-реквестов, 
но pull request это по сути всего лишь письмо электронной почты, поэтому github можно заменить 
на что угодно вплоть до перехода на собственные git-сервера в p2p-сети.

Не обязательно каждому участнику забирать изменения абсолютно у всех участников.

У github есть свой клиент, который может быть удобен для синхронизации с репозиториями участников
(забора изменений) и создания pull-реквестов из их репозитория в свой (чтобы забрать их изменения себе).
Кроме того прямо с сайта github можно склонировать чужой репозиторий в папку на своём компьютере.

Большей гибкости можно достигнуть при использовании консоли, что имеет и возмжоности для автоматизации
большинства действий. Это мы и рассмотрим в этой инструкции.

Рассмотрим пример добавления себе нового репозитория для синхронизации в ручном режиме 
(все нижеописанные действия могут быть сделаны полуавтоматическими с помощью нумерации, 
интерактивно - смотрим изменения, отклоняем либо принимаем, для этого можно подготовить
скрипт, это не обязательно, но должно быть удобно):

    mkdir zeptoboard-35
    cd zeptoboard-35
    git clone ...здесь адрес нового репозитория...
    cd ..

переходим в свой родной репозиторий

    cd zeptoboard-0/zeptoboard
    
это делается один раз:

    git remote add -f z35 ../zeptoboard-35/zeptoboard

через время находясь в родном репозитории скачиваем все изменения со всех репозиториев одной командой

    git remote update
    
затем смотрим по каждому какие отличия от нашего, на примере 35-го:

    git diff master remotes/z35/master
    ...здесь будут показаны добавления зеленым и удаления красным
    стоит отметить что никаких удалений быть не должно в принципе
    кроме удалений из repo.txt
    никто не вправе удалять у себя (а значит и у всех) какой-либо пост
    и оформлять это в виде коммита. если вам не нравится пост - 
    просто не принимайте его изначально

если изменения нас устраивают, берём их себе:

    git merge remotes/z35/master
    
если видим спам или еще что-то, с чем не желаем мириться, удаляем репозиторий из списка:

    git remote rm z35
    
и из файловой системы:

    rm -r ../../zeptoboard-35
    
периодически заходим на github.com, чтобы подтвердить или отклонить pull-request-ы
с добавлением ссылок на репозитории в файл repo.txt - других pull-request-ов быть не должно!
ссылка должна совпадать с репозиторием с которого идёт pull-request.
осмотрите репозиторий, если он не кажется вам подозрительным, добавьте его себе по инструкции выше.

Некоторые ссылки могли прийти вам от других людей. Вашей ссылки по тем ссылкам может не быть. 
Поэтому стоит иногда заходить на такие репозитории и делать pull-request, предлагая свой репозиторий.

Свои посты вы пишете в своем репозитории, а для сохранения анонимности, чтобы не ясно было где ваш пост, 
а где пост, который вы получили в результате операции merge из другого репозитория, стоит сразу же
выполнять слияние всех изменений в один коммит, способов несколько:
http://stackoverflow.com/a/24154149
(потребуется force push)

И только после этого делать git push из своего репозитория.

Работа со скриптом, который автоматизирует все эти действия может выглядеть так:

    $ ./zb add https://github.com/anon-78213479/zeptoboard.git
    cloning... added to list!
    $ ./zb update
    Starting interactive mode. 
    ###################################
    Showing differences with https://github.com/anon-78213479/zeptoboard.git:
    diff --git a/crypt/2016/my_thread1.txt b/crypt/2016/my_thread1.txt
    привет,
    это мой тред
    +--
    +отвечаю
    @@ -0,0 +1 @@
    ###################################
    Commands: m - merge, d - decline, r - remove repo from list
    >m
    Merging...
    ###################################
    Showing differences with ...
    
Подытожим, что имея настольный клиент github и пользуясь лишь этим клиентом 
и сайтом github всё еще можно полноценно пользоваться зептобордой. Но консоль
и скрипты будут быстрее, удобнее, меньше ручной работы и больше возможностей 
в плане анонимности. А для просмотра разницы можно подключить красочные оконные 
diff-утилиты и показывать их прямо из скрипта, так что вид diff-ов будет не хуже
чем в клиенте или на сайте github.
