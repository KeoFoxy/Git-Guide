# Git Guide  
### _Ссылки на альтернативные статьи_  
[**Статья по Git на Habr**](https://habr.com/ru/post/273897/)  
[**Статья по Git на HTML-Academy**](https://htmlacademy.ru/blog/useful/git/git-console)  
[**Git и GitHub Курс для Новичков**](https://youtu.be/zZBiln_2FhM)

## _Инструкция по использованию GIT_  
[**Если вы не программист и не хотите заморачиваться с консолью, то вам сюда**](#github-desktop)  
[**GitHub для Visual Studio Code**](#githib-в-visual-studio-code)  
[**Список основных команд**](#список-основных-команд)  
### Установка GIT
Для начала нам необходимо уставноить сам Git на устройство. Сделать этом можно на [официальном сайте](https://git-scm.com/download/win). Рекомендуется использовать VPN при скачивании, тк может быть очень низкая скорость загрузки для российских пользователей.
Для пользователей **MacOS** установка выполняется через терминал следующим образом (необходимо иметь менеджер пакетов [brew](https://brew.sh/index_ru)):  
`$ brew install git`  
Для пользователей **Linux** существует несколько способов:  
* `apt-get install git` для Debian/Ubuntu
* `yum install git` Fedora 21 или ранее
* `dnf install git` Fedora 22+

Чтобы проверить, установлен ли Git на вашем устройстве — необходимо открыть терминал и прописать `git --version`. Если все успешно, то терминал выведет вам версию вашего Git'a.

### Настройка Git

После установки Git необходимо его настроить.  
Открываем терминал и вводим команду:  
`git config --global user.name "Ник"`.  *Пример `git config --global user.name "ZeroTwoTopWaifu"`*  
Следующая команда:  
`git config --global user.email почта`.  *Пример `git config --global user.email VT_Plis@gmail.com`*  

Обратите внимание, что в командах, указанных выше, есть опция `--global`. Это значит, что такие данные будут сохранены для всех ваших действий в Git и вводить их больше не надо. Если вы хотите менять эту информацию для разных проектов, то в директории проекта вводите эти же команды, только без опции `--global`.

### Регистрация на GitHub

Переходим на [сайт Github](https://github.com/) и регистрируемся. Не забудьте подтверlить почту.  
Поздравляю! Теперь у Вас есть аккаунт на GitHub. После регистрации и подтверждения почты, гитхаб сам предложит создать репозиторий. 

### Конфигурация SSH-ключей

Что такое **SSH-ключ** и зачем он нужен?  

Чтобы работать со своего компьютера с GitHub, иметь доступ к проектам, хранящимся на сервисе, выполнять команды в консоли без постоянного подтверждения пароля, нужно пройти авторизацию у сервера. В этом помогают SSH-ключи.  

Каждый SSH-ключ содержит пару: открытый (публичный) и закрытый (приватный) ключ. Открытый ключ отправляется на сервер, его можно не прятать от всех и не переживать, что кто-то его увидит и украдёт. Он бесполезен без своей пары — закрытого ключа. А вот закрытый ключ — секретная часть. Доступ к нему должен быть только у вас.  

Вы отправляете какую-то информацию на сервер, где хранится ваш публичный ключ, сервер понимает, что вы это вы, то есть идентифицирует именно вас, и даёт вам какой-то ответ. И только вы можете расшифровать этот ответ, потому что только у вас есть подходящий закрытый ключ. Получается что-то вроде связки логин-пароль только намного безопасней. Ваш пароль кто-то может узнать или подобрать, а чтобы получить ваш приватный SSH-ключ, злоумышленнику придётся взломать ваш компьютер.  

Чтобы пройти авторизацию по SSH-ключу, его надо сгенерировать или найти уже ранее созданный ключ на своём компьютере.  

После небольшой теории приступаем к практике.  
P.S В данной инструкции в командах будет присутствовать название файла **id_rsa**, у вас название этого файла может быть другим. 
1. Первым делом проверим наличие на компьютере ключа. Ключи хранятся в каталоге `~/.ssh`, поэтому проверим содержимое каталога.
2. Открываем консоль и вводим `cd ~/.ssh`. Мы попадаем в каталог. Далее прописываем `ls`, чтобы увидеть все файлы внутри.
 
![files-check](pic/mac_git_key_checked.jpg)

Если ключи уже имеются на устройстве, то мы увидим несколько файлов, среди которых будут `name` `name.pub`. Они могут называется по-разному — `id_rsa`, `id_dsa`,`id_ecdsa`,`id_ed25519`. Файл с расширением `.pub` — публичный ключ, второй — привытный.

Если у вас нет таких файлов или вообще нет директории `.ssh`, то их необходимо сгенерировать.  

![no-files](pic/NoKey.PNG)  
  
Открываем консоль и вводим команду:  

`ssh-keygen -t rsa -b 4096 -C "your_mail@example.com"`  *— Здесь необходимо указать ту почту, которую использовали для регистрации на GitHub.*  

![key-generation](pic/KeyGeneration.PNG)  

Далее вас попросят указать расположения директории для сохранения ключа. Можете ввести путь к своей директории или нажать `Enter` и файл сохранится по адресу, указанному в скобках.  

После вас попросят дважды ввести пароль к ключу. Если не хотите вводить пароль каждый раз, когда используете ключ, пропустите этот шаг, нажав `Enter`, и ничего не вводите.  

Теперь у вас сгенерирован ключ по указанному выше адресу.  

![key-generated](pic/KeyGenerated.PNG)  

Добавляем ключ в `ssh-agent`(сгенерированный или уже существующий). Проверяем доступность ключа командой:   
`eval "$(ssh-agent -s)"`  

Если на Windows возникают проблемы, то используйте команду: `start-ssh-agent`, если и это не помогло, то используйте **Git Bash**.  

Добавляем ключ `ssh-add ~/.ssh/Имя ключа` *Пример `ssh-add ~/.ssh/id_rsa`*  

**Несколько важный примечаний:**
- Если вы захотите переименовать ключ, то могут возникнуть проблемы. Решаются с помощью связи ключа с доменом. `~/.ssh/config`  
- Если у вас MacOS Sierra версии 10.12.2 и выше, нужно изменить ваш `~/.ssh/config` файл, чтобы автоматически загрузить ключи в ssh-agent и хранить пароли  
```
Host *
 AddKeysToAgent yes
 UseKeychain yes
 IdentityFile ~/.ssh/id_rsa
 ```
 Вы можете добавить свой приватный ключ в **ssh-agent** и сохранить пароль к нему с помощью команды `ssh-add -K ~/.ssh/id_rsa`. Если у вашего ключа другое имя, не забудьте заменить **id_rsa** в команде на правильное название.  
 - Если у вас Linux, может понадобится переназначить для **~/.ssh** права доступа командой `chmod 700 ~/.ssh/`.   

После того, как мы создали ключ, его необходимо добавить на **GitHub**.  
Скопировать ключ можно разными методами:  
Самый простой: открыть файл **id_rsa** через блокнот или Notepad++ и скопировать содержимое.  
Или используя команды:  
- Windows: `clip`
- MacOS: `pbcopy`
- На Linux используйте `sudo apt-get install xclip`, чтобы установить пакет **xclip**, а затем введите `cat ~/.ssh/id_rsa.pub` (или какое там у вас название файла.) Содержимое файла появится в консоли и вы сможете его оттуда скопировать.

**После того, как мы скопировали ключ**

Переходим на [страницу для работы с ключами](https://github.com/settings/keys) в вашем профиле на GitHub.  
Нажимаем кнопку New SSH key(Новый SSH ключ). Вводим название для ключа(Абсолютно любое название) в поле Title, ниже вставляем наш ключ из файла. Добавляем ключ. Вводим пароль от учетной записи и после у вас в списке появится ключ.  
Поздравляю! Настройка завершена.  


### Работа с репозиториями
После создания репозитория на гитхабе его необходимо клонировать себе на устройство. Предварительно необходимо перейти в директорию, где вы хотите хранить этот репозиторий (В консоли: `cd 'Путь или название папки'` или используйте **GitBash** в нужной папке.   
Клонируем репозиторий с помощью команды:  
`git clone git@github.com:NickName/your-repository.git` *Пример `git clone git@github.com:KeoFoxy/test_git.git`*  

**В GitBash не работает стандартное ctrl+c ctrl+v, используйте shift+insert.**  

Если вдруг появилось сообщение об ошибке `Error: Permission denied (publickey)`, то значит, что вы что-то сделали не так, вернитесь к инструкции выше и попробуйте заново.  

Если хотите, чтобы название папки с репозиторием отличалось от имени репозитория, то используйте:  
`git clone git@github.com:_your-nickname_/_your-project_.git folder_name`  

После клонирования репозитория необходимо попасть в его директорию. В начале командной строки в консоли у вас указывается адрес, где вы сейчас находитесь.  

При работе над проектом используют ветки. У репозитория есть по умолчанию одна ветка **main**, в ней хранится основная рабочая версия проекта. Чтобы исправить что-то или добавить и не сломать код, добавьте ветку в проект. В новую ветку скопирются все файлы и вы можете спокойно делать с ними все, что угодно.  

Чтобы узнать состояние ветки, используйте команду: `git status`  
Она покажет текущее состояние в вашей ветке, а именно список с названиями изменённых файлов, если они есть, и укажет на те, которые ожидают записи и сохранения (обычно они выделены красным цветом).  
Перед тем, как зафиксировать изменения отдельных файлов, нужно добавить файлы в набор этих изменений. Воспользуйтесь командой `git add имя-файла`  
Если вы хотите сохранить все изменения разом, вводите `git add -A`  

После добавления файла, мы можем сделать коммит, зафиксировать изменения и написать комментарий к ним:  
`git commit -m "Ваше сообщение"`  
Пишите осмысленные комментарии к коммитам, что вы сделали в этом изменении и тд.  

После того, как коммит сделан, можно отправлять наши изменения в репозиторий на GitHub:  
`git push origin название-текущей-ветки` *Пример `git push origin Master`*  

Готово! Наши изменения улетели в ветку Master и теперь они находятся в репозитории на GitHub!  

Теперь рассмотрим такую вещь, как **Pull Request**.

![pull-request](pic/PullReq.PNG)  

Когда вы или ваш коллега закончите работу в новой ветке, не основной, и отправите изменения на ГитХаб, то будет создан запрос на слияние, где администратор репозитория может его рассмотреть и принять или отклонить изменения. Если изменения устраивают, то необходимо сделать **Merge** веток, то есть слиять их.   

![pull-request-open](pic/OpenPullReq.PNG)   

Любое предложение можно принять или отвергнуть. Так же и с пулреквестом. После его создания, он должен получить ревью и одобрение так называемого коллаборатора — пользователя GitHub, который имеет права администратора в мастер-репозитории. Им может быть ваш коллега-разработчик, техлид, наставник. Если к вашему коду нет вопросов, пулреквест принимается и изменения из вашей ветки попадают в `main` главного репозитория. Если в код нужно внести изменения, пулреквест отклоняется, и вам нужно снова пройти по цепочке локальные изменения — сохранение — коммит — пуш, только пулреквест заново делать не нужно. Если вы продолжаете вести работу в той же ветке и пулреквест ещё не принят, все ваши изменения автоматически добавятся в пулреквест, созданный из этой ветки после команды `git push origin название-текущей-ветки`  

Pull Request показывает, что изменилось в новой ветке, по сравнению с основной, если нет никаких конфликтов, то вы спокойно можете делать слияние.  

Теперь рассмотрим случай, когда необходимо обновить ваш репозиторий до актуального. Ведь кто-то мог также параллельно работать с репозиторием и вносить изменения, а у вас их нету. Для этого используем команду:  
`git pull название_репозитория Название ветки` *Пример `git pull test_git Master`*  
Если выскакивает ошибка, то используйте:   
`git remote add название_репозитория git@github.com:your-repo.git` 
а затем снова  
`git pull название_репозитория Название ветки`   
*Пример `git pull test_git git@github.com:KeoFoxy/test_git.git`*  

Готово! Теперь ваша копия и оригинальный репозиторий находятся в актуальном состоянии.  

### Список основных команд:  
`cd имя-папки-или-путь` - попасть в директорию по адресу или названию  
`git clone git@github.com:your-nickname/your-project.git` - Клонировать репозиторий   
`git checkout -b имя-новой-ветки` - Создать новую ветку  
`git checkout название_ветки` - Переключиться на ветку  
`git branch -m старое-имя-ветки новое-имя-ветки` - Сменить название ветки  
`git status` - проверить статус ветки  
`git add -A` - добавить все файлы в изменения  
`git add имя-файла` - добавить файл в изменения  
`git commit -m "Ваше сообщение"` - создать коммит  
`git push origin название-текущей-ветки` - отправить изменения в репозитория на GitHub  
`git pull название-репозитория название ветки` - синхронизировать ваши файлы на устройстве с репозиторием на GitHub  
`git remote add название-репозитория git@github.com:your-repo.git` - добавить ссылку на репозиторий  

### GitHub Desktop  
Если работа с консолью и командами вызывает у вас страх, а программистом вы не планируете становится, то для вас есть решение — [GitHub Desktop](https://desktop.github.com/).  

![GitHub-Desktop-Menu](pic/GitHub_Desktop_menu.PNG)

Данное приложение невероятно упрощает работу с репозиториями.  
Теперь расскажу, как работать с приложением.  
1. Чтобы клонировать репозиторий, создать новый или добавить уже существующий с устройства, необходимо выбрать **File** на панели сверху. В выпадающем меню выбрать нужный вариант. После у вас чуть ниже появится репозиторий в текущих. Можно использовать множество репохиториев и переключаться между ними. 
2. Под названием текущего репозитория и идут 2 вкладки: изменения и история. В изменениях показаны те файлы, в которых что то поменялось, на каждый файл можно переключиться. Зеленым показано то, что в файле добавилось, а красным - что удалили.  
![GitHub-Desktop-Changes](pic/GitHub_D_Changes.PNG)  
3. Перейдем к следующим двум вкладкам: выбор веток и push/pull.  
![GiHub-Desktop-Bar](pic/GitHub%20bar.PNG)  
Current branch отвечает за выбор ветки. Из выпадающего меню можно быстро перейти в другую ветку, если у вас были какие то изменения в текущей, то вам предложат оставить их в ней или перенести новую/выбранную ветку.  
![Branch](pic/BranchMenusm.PNG)  
4. Теперь вернеся обратно к файлам. Автоматически GitHub Desktop выделяет все файлы для коммита, но вы можете снять галочки и выбрать только нужное. Под списком файлов находится поле summary — это поле для сообщения, которые необходимо написать для коммита. Пишите что-то осмысленное! Что вы поменяли, создали, исправили и тд. Также можно создать описание - это не обязатеольно.  

![commit](pic/Commit.PNG)   
**Теперь внимание!!!**  
У Git есть такая вещь как **.gitignore** — это конфиг-файл, в который добавляются расширения файлов, которые не будут отслеживаться и добавляться в репозиторий.  
*Например: если добавить .cpp в конфиг, то все файлы с таким расширением не будут проверятся системой Git, то есть она не будет отслеживать изменения вних и добавлять в репозиторий.*  
Ниже приведен пример **gitignore** файла. Как мы видим, хоть в папке и присутствуют другие файлы, но система не отслеживает их в приложении, потому что их расширения в gitignore.  

**Добавить файл или файлы в gitignore можно следующим образом: ПКМ по файлу и выбираете ignore file или ignore all .() - все файлы с таким расширениями улетят в gitignore.**  

![gitignore](pic/gitignore.PNG)
При работе с лабами по ПЛИС Quartus создает еще много файлов, которые нам могут быть не нужны в репозитории. Рекомендуется добавить все эти файлы и папки в .gitignore, а оставить только .v .vwf - файлы Verilog и файлы временных диаграмм.  

5. После того, как мы написали осмысленный коммит и нажали **commit to (ваша ветка)** мы можем отправить изменения в репозиторий. Для этого на панели рядом с выбором веток есть кнопка **Push origin**  
![push](pic/push_origin.PNG)  
После нажатия ваши изменения появятся на GitHub.
Если вы вносили изменения с другого устройства или кто-то другой добавлял файлы в репозиторий, то эта же кнопка обновляет проект до актуального. **Pull origin**.  
![pull](pic/PullOrigin.PNG)  
Если нет кнопки Pull origin, то нажмите **Fetch Origin**  
![fetch](pic/FetchOr.PNG)  

6. Еще из полезных функций стоит выделить откат изменений. Если вдруг вы что-то изменили так, что все перестано работать, то можно нажать **Discard changes**(ПКМ по файлу) и файл вернется в состояние до последнего коммита.

### GitHib в Visual Studio Code  

**!Важный момент!**  
Если вы впервые будете использовать GitHub через Visual Studio Code, то надо будет прописать в терминале конфигурацию Git для избежания проблем в дальнейшем. На панели сверху выбираем **Termial** > **New Terminal** или нажимаем **Ctrl+Shift+`** и прописываем 2 команды:  

`git config --global user.name "Ник"`.  *Пример `git config --global user.name "ZeroTwoTopWaifu"`*   
`git config --global user.email почта`.  *Пример `git config --global user.email VT_Plis@gmail.com`*  
  
Чтобы работать с GitHub через редактор кода Visual Studio Code не потребуется много усилий. Прямо со стартового окна можно клонировать репозиторий с гитхаба и начать работу с ним. Все что нам потребуется - залогиниться в свой аккаунт гитхаба. 
![VSC](pic/VSCode.PNG)  
Или можно перейти в раздел **Source Control** на панели слева.  
![VSCPanel](pic/VSCPanel.PNG)  
Все наши изменения будут отображаться там. Чтобы сделать коммит, необходимо всего лишь выбрать нужные файлы или сразу все, нажав на плюсик:  
![VSCChanges](pic/VSCodeChanges.PNG)  
После того, как вы выбрали нужные файлы, необходимо написать сообщения для коммита. После этого нажать на галочку - она сделает commit.  
![VSCCheck](pic/VSCCheck.PNG)  
Отправить наши изменения на GitHub можно несколькими способами:
* нажать на ... и во всплывающем окне выбрать Push.  
![VSCdots](pic/VSCdots.PNG)
* спуститься в низ и нажать на значок синхронизации, он же используется для приведения локального репозитория в актуальное состояние(Pull)  
![VSCbottom](pic/Sync_bottom.PNG)  
* Если все изменения обработаны, то можно нажать на появившуюся кнопку:  
![SyncBytton](pic/Sync.PNG)  

Готово! Наши изменения появились на GitHub. 