[Я не туда нажал и хочу обратно в оглавление](./000%20toc.md)

# Настроим ssh-сервер на Ubuntu

На этой странице мы планируем изучить следующие вещи:

1. Команды для обновления ПО на линукс.
2. Установка нового ПО (ssh-сервер).
3. Проверка работоспособности ssh-сервера при помощи ssh-клиента из локальной Windows машины.

Выглядит довольно-таки выполнимо.

## Прежде чем ставить какое-то ПО

Так вот, прежде чем ставить какое-то по на линукс, необходимо получить все обновления для программ и библиотек, которые на данный момент установлены, чтобы все версии всего, что там у вас уже понаставлено были самые последние и поддерживались бы тем софтом, который вы ставите.

Почему? Ну, предположим у вас стоит библиотека lib_3_2_1, которая используется тем софтом, который мы ставим. Софт, который мы ставим будет по умолчанию последней версии. Есть ненулевая вероятность, что софту будет нужна версия библиотеки lib_5_6_7, которая умеет то, что не умеет lib_3_2_1. Опять же есть ненулевая вероятность, что если мы не будем иметь библиотеку нужной версии, устанавливаемое ПО попросту не будет работать и мы проведем незабываемые часы в разбирательствах типа "какого хера", хотя ларчик просто открывался.

### Ты еще не сказал, как его ставить-то

![picture](./img/008_stupidJoke.jpg)

Ah, alright.

В убунте через командный интерфейс (CLI - command line interface) для установки и обновлений ПО имеются утилиты (или команды?) apt, apt-get. Первая позиционируется как apt-get с человеческим лицом. Более высокоуровневая, blah-blah-blah. Для наших целей неважно, какую использовать, я привык использовать ~~фзе-пуе~~ ```apt-get``` даже несмотря на то, что она длиннее, поэтому все примеры будут с этой командой, но все они также будут работать и с ```apt```. 

#### Обновляемся
В окне терминала пишем (это одна строка):

```sudo apt-get update && sudo apt-get upgrade```

В терминале начнется некоторое бурление, иногда терминал будет спрашивать разрешение на установку, чтобы оно прошло, нужно нажимать ```Y``` и потом Enter.

#### Устанавливаем openssh сервер

Терминал:

```sudo apt-get install openssh-server```

>Мы только что выучили новую опцию для **apt/apt-get**, которая позволяет устанавливать софт: ```install```

Как всегда после этого будет штук 50 строк, которые будут нам рассказывать о том, что на данный момент происходит.

#### Проверяем, а правда ли установился openssh сервер

Терминал:

```sudo systemctl status ssh```

Должно появиться что-то типа вот этого:

![picture](./img/008%20SshCheckRunningResult.png)

Чтобы выйти, нужно нажать ```Ctrl+C```

### Пытаемся соединиться с Ubuntu по ssh из git bash
Окно git bash на вашей windows машине:

```ssh user@192.168.1.xxx```

**user@** позволяет нам сразу ввести имя пользователя и ssh-сервер будет нас запрашивать только пароль

При первом соединении ssh агент на вашей машине с подозрением отнесется к тому серверу, куда вы пытаетесь подключиться, но нужно его успокоить и сказать ```yes``` и далее ввести пароль.

Далее, если вы, конечно, помните пароль, вы увидите Вэлкамэ сообщение:

![git bash 1st connect](./img/008%20SshGitBash1stConnect.png)

### Пытаемся соединиться с Ubuntu по ssh из Putty

Для тех, кто забыл:

![Putty 1st time](./img/008%20SshConnect1stTimePutty.png)

Putty тоже сначала с подозрением отнесется к серверу, и нужно дать свое согласие.

![Putty 1st time](./img/008%20SshPuttyYes.png)

И потом пароль и Вэлкамэ.

![Putty 1st time](./img/008%20SshPuttyLogin.png)

> Начиная с этого момента вот это окно нам больше не нужно:

![Ubuntu desktop](./img/008%20SshUbuntuUselessDesktop.png)

Все последующие действия на Ubuntu мы будем делать или из Putty или из gut bash — т.е. будем эмулировать удаленный Linux сервер (VPS), на котором мы настраиваем нашу среду для автотестов.

>Что дальше: настраиваем доступ по публичному ключу

Этот вариант аутентификации для ssh является еще более безопасным, чем просто использование ssh с именем пользователя и паролем.

### но пока...

или в Putty или в git bash вводим:

```sudo shutdown ```

Ждем минуту, когда ubuntu остановится.

Переходим в основное окно VirtualBox и запускаем виртуальную машину в headless режиме вот так:

![VM headless start](./img/008%20SshVboxHeadlessStart.png)

Теперь окно виртуальной машины не появится и работать мы будем только через терминал локальной Windows машины.

Рекомендую еще отключить просмотр (справа на картинке: Preview > Update disabled), а понять, что она работает можно вот так:

![VM Status](./img/008%20SshVMStatus.png)

Ну, всё. Теперь мы как бы работаем с удаленным VPS.

Погнали дальше.

- [x] [Создадим виртуальную машину.](005%20vm%20and%20ubuntu.md)
- [x] [Установим на нее Ubuntu.](005%20vm%20and%20ubuntu.md)
- [x] [Проверим, что все установилось.](006%20checkWeAreOkay.md) 
- [x] [Да! Еще выучим пару команд линуха.](006%20checkWeAreOkay.md)
- [x] [Настроим ssh на локальной Windows машине.](007%20sshLocalWindows.md)
- [x] [Настроим ssh сервер на Ubuntu.](008%20sshOnVm.md) :eggplant: Here
- [ ] [Настроим аутентификацию при помощи публичного ключа на Ubuntu.](009%20ssh-passwordless.md)
- [ ] Начнем работать с Ubuntu только через терминал.
- [ ] Установим JDK.
- [ ] Установим SDK.
- [ ] Установим Gradle.
- [ ] Установим Jenkins.
- [ ] Установим Docker.
- [ ] Установим Selenoid.


[Оглавление](./000%20toc.md)