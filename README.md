# Настройка собственного VPN-сервиса за 5 минут
Эта инструкция предназначена для тех, кто хочет оставаться на связи в заблокированных соцсетях, а также иметь доступ 
более-менее реальной информации, которая не испорчена российской пропагандой и не похожа на произведения Оруэла. 
Самый простой способ сейчас - купить подписку на платные VPN-сервисы. Бесплатными пользоваться КРАЙНЕ НЕ РЕКОМЕНДУЮ, 
т.к. от них можно получить огромную кучу проблем.
Я, например, использую платный сервис [HideMyName](https://hidemy.name/#62399142d25c7).
У платных сервисов есть, как преимущества, так и недостатки.

### Преимущества платных сервисов VPN:
- Простота установки и настройки - свои приложения для различных устройств и платформ.
- Возможность выбора страны и города расположения сервера VPN
- Как правило, с одним аккаунтом можно пользоваться с нескольких устройств.
### Недостатки платных сервисов VPN:
- Платный, хотя это так себе недостаток - есть за что заплатить.
- Российские власти активно борются с VPN-сервисами и многие уже заблокированы в России, т.е. есть вероятность того, что 
просто потеряете свои деньги, если сервис заблокируют.
- Хоть сервисы и заявляют, что не сохраняют ваши данные и никому их не передадут, в т.ч. по запросу правоохранительных 
органов, гарантировать на 100% этого нельзя.
- Учитывая то, что сервисом пользуется помимо вас, еще много человек, может страдать скорость доступа.

В этой статье я постараюсь максимально подробно и пошагово объяснить, как сделать собственный VPN-сервис, который будет 
также работать на всех устройствах и платформах, при этом вы будете на 100% уверены, что ваши данные никуда не попадут 
без вашего на то желания. Это будет ваш личный VPN-сервис и пользоваться им будете только вы, либо те, комы вы сами 
предоставите доступ.
## Несколько важных моментов:
- Эта инструкция про IT, а не про нарушение законодательства
- Использовать любой VPN, будь то сомнительный бесплатный, платный, либо собственный, для противоправных действий 
нельзя, т.к. хоть речь и пойдет про серверы в иностранных юрисдикциях, которые в текущих реалиях вряд ли пойдут 
навстречу российским правоохранительным органам, но все же, это не про анонимность, а про обход блокировок. Анонимность 
достигается за счёт немного других средств и не относится к теме данной статьи.

# Итак, поехали!
## Весь процесс состоит из 6 простых шагов:
1. Аренда сервера (VPS)
2. Скачивание необходимых приложений
3. Подключение к серверу и настройка сервиса VPN
4. Добавление пользователей VPN
5. Копирование файлов конфигурации подключения на устройства
6. Подключение к VPN.

#### Аренда сервера (VPS)
Для настройки собственного VPN-сервиса нам необходимо арендовать виртуальный сервер с операционной системой Linux. Такая 
услуга называется VPS (англ. virtual private server). При этом, нам необходимо арендовать сервер, расположенный стране, 
где доступ к необходимым ресурсам не блокируется. Я предпочитаю использовать серверы в Германии.

В России _пока ещё_ есть компании, которые могут предоставить сервер на зарубежных площадках. Ключевое слово _"пока"_. 
Один из таких, которым я пользуюсь и рекомендую - [RuVDS](https://bit.ly/RuVDSForVPN). В данном контексте его основным 
преимуществом, пожалуй, будет возможность оплаты в рублях и всеми доступными в России способами. К недостаткам я бы 
отнёс неопределённость будущего в части доступности для аренды зарубежных площадок. На момент написания данной статьи 
остались доступны для аренды серверы только на одной из площадок в Германии, а, например, в Великобритании и Швейцарии 
уже недоступны. 

Другой вариант - арендовать сервер напрямую на зарубежной площадке. Однако здесь возникает проблема оплаты - российские 
карты Visa, Mastercard и Мир там не работают. Но решение есть! Карта UnionPay. Стоимость выпуска пластиковой карты 
UnionPay стоит, на мой взгляд, овердофига - до 9500₽, но в Почта-Банке можно бесплатно открыть сберегательный счёт, а 
затем в приложении также бесплатно выпустить виртуальную карту UnionPay. К сожалению, наша пропаганда и здесь преуспела, 
сказав, что эти карты принимают повсеместно и они чуть ли не более распространены, чем Visa и Mastercard. Короче, чтобы 
найти сервис, который их принимает, нужно очень постараться. 
Я нашел немецкий [BlueVDS](https://bit.ly/BlueVDSForVPN).

Необходимо арендовать сервер с операционной системой Linux Ubuntu (для других версий Linux я дополню инструкцию позже). 
Если предполагается к использованию малого количества пользователей, (думаю, до 10 юзеров, но это пальцем в небо), то 
достаточно самой минимальной (и самой дешевой) конфигурации. 

После регистрации сервера, вам будет выдан `IP-адрес сервера`, а также `пароль для пользователя root` (это пользователь с 
полными правами доступа к серверу). IP-адрес будет использоваться для настройки сервера, а также непосредственно для 
подключения к VPN-сервису (он же будет определяться ресурсами, доступ к которым вы будете получать). Также в некоторых 
случаях может быть предоставлен номер порта подключения. Например, в случае, с [BlueVDS](https://bit.ly/BlueVDSForVPN),
вместо стандартного порта `22`, используется порт `56777`. Если порт не указан, то используется порт по-умолчанию `22`! 

Видео процесса регистрации (аренды) сервера в [RuVDS](https://bit.ly/RuVDSForVPN) можно посмотреть здесь, здесь или 
здесь, а для [BlueVDS](https://bit.ly/BlueVDSForVPN) здесь, здесь или здесь.

#### Скачивание необходимых приложений
1. Для начальной настройки сервера понадобится специальная программа - SSH-клиент. Если вы настраиваете сервер, используя 
компьютер с Windows, то необходимо скачать с официального сайта бесплатную программу 
[Putty](https://the.earth.li/~sgtatham/putty/latest/w64/putty-64bit-0.76-installer.msi). В случае же, если вы используете 
MacOS, то ничего дополнительно скачивать не придется - все необходимое уже есть.
2. Для непосредственного подключения к VPN-сервису, необходимо скачать и установить бесплатную программу VPN-клиент OpenVPN.
На официальном сайте [OpenVPN](https://openvpn.net/community-downloads/) можно скачать приложения для всех популярных 
платформ. 
Для мобильных устройств бесплатные приложения есть в [AppStore](https://apps.apple.com/us/app/openvpn-connect/id590379981) 
и [GooglePlay](https://play.google.com/store/apps/details?id=net.openvpn.openvpn&hl=ru&gl=US)

#### Подключение к серверу и настройка сервиса VPN
На данном этапе приступим к непосредственной настройке VPN-сервиса на нашем сервере. Здесь нам понадобятся `IP-адрес` и 
`пароль root-пользователя`, которые мы получили на при регистрации сервера на первом шаге.

В зависимости от того, какой компьютер мы используем для настройки (c Windows или с MacOS), отличаться будет только первый 
шаг - способ подключения к серверу.

###### Внимание!!! Все данные необходимо вводить в точном соответствии с регистром!!! 
###### Например, `root` - правильно, а `Root` - неправильно!

##### Для Windows:
1. Запустите установленную программу [Putty](https://the.earth.li/~sgtatham/putty/latest/w64/putty-64bit-0.76-installer.msi)
2. В поле `Host Name (or IP address)` укажите имя пользователя `root`, после него символ `@` и далее выданный IP-адрес. 
Например, если выдан IP-адрес _123.123.123.123_ должно получиться `root@123.123.123.123`
3. В поле `Port` укажите номер выданного порта (если есть). Если номера порта нет, оставьте по-умолчанию `22`
4. Нажмите кнопку `Open`

##### Для MacOs:
1. Запустите стандартную программу `Terminal`. (Нажмите клавиши `Ctrl` + `Пробел` и наберите _terminal_ или _терминал_). 
Появится окно терминала. 
2. Введите строку ssh root@<ip-адрес> -p <порт>. Например, если выдан IP-адрес _123.123.123.123_, а порт _56777_ должно 
получиться: 


    `ssh root@123.123.123.123 -p 56777`

3. Нажмите `Enter`

Дальнейшие действия для Windows и MacOs одинаковые.

Введите выданный пароль пользователя `root` и нажмите `Enter`. _Вводимые символы отображаться не будут._

При успешной авторизации отобразится приветствие, начинающееся с `root@`

Скопируйте и вставьте следующую строку, после чего нажмите `Enter`:

    apt update && apt upgrade -y && apt install curl -y && curl -fsSL https://get.docker.com -o get-docker.sh && sh ./get-docker.sh && systemctl enable docker && systemctl start docker && docker pull kylemanna/openvpn && docker volume create --name openvpn && docker run -v openvpn:/etc/openvpn --rm kylemanna/openvpn ovpn_genconfig -u udp://$(curl -4 icanhazip.com) && docker run -v openvpn:/etc/openvpn --rm -it kylemanna/openvpn ovpn_initpki nopass && docker run -v openvpn:/etc/openvpn -d -p 1194:1194/udp --cap-add=NET_ADMIN kylemanna/openvpn

Процесс установки займет несколько минут. После успешной установки также появится приветствие, начинающееся с `root@`

#### Добавление пользователей VPN
На следующем шаге необходимо добавить пользователей VPN-сервиса. Рекомендую добавлять отдельных пользователей для разных 
устройств. Например, `ivan_pc`, `ivan_iphone`, `ivan_router` и т.д. 

Для добавления пользователя введите следующую команду, заменив вначале значение LOGIN на необходимое имя пользователя. 
Используйте символы латинского алфавита, цифры, тире и знаки подчеркивания. Например, для пользователя marina введите: 

    LOGIN="marina" && docker run -v openvpn:/etc/openvpn --rm -it kylemanna/openvpn easyrsa build-client-full $LOGIN && docker run -v openvpn:/etc/openvpn --rm kylemanna/openvpn ovpn_getclient $LOGIN > $LOGIN.ovpn

В процессе создания пользователя, система попросит задать и повторить пароль - это пароль пользователя, с которым он 
будет подключаться к VPN-сервису.

_Повторите данную команду для создания всех пользователей, меняя логин._

После успешного выполнения команд на сервере будут созданы конфигурационные файлы, необходимые для подключения. Например,
_marina.ovpn_, _ivan_iphone.ovpn_ и т.д. Посмотреть список созданных файлов можно с помощью команды: 

    ls

На этом настройка сервера завершена. Для завершения работы с сервером необходимо ввести команду:

    exit

#### Копирование файлов конфигурации подключения на устройства.
Для подключения к сервису VPN необходимо скопировать с сервера созданные конфигурационные файлы на устройства, с которых 
будет осуществляться подключение к VPN-сервису. 

Для начала эти файлы необходимо скопировать на локальный компьютер. 

Для этого на локальном компьютере необходимо выполнить следующие действия:
##### Для Windows, отройте командную строку и выполните команду:
    pscp.exe -P <port> root@<ip-address>:/root/*.ovpn <folder>
Замените <ip-address> на IP-адрес сервера, <port> - порт подключения, а <folder> на путь к папке, в которую необходимо сохранить файлы. Папка должна 
существовать. Вместо пути к папке можно поставить точку - тогда файлы сохранятся в текущую папку. Например: 

    pscp.exe -P 22 root@123.123.123.123:/root/*.ovpn .

или

    pscp.exe -P 56777 root@123.123.123.123:/root/*.ovpn "c:\vpn"


##### Для MacOS, отройте терминал и выполните команду:

    pscp.exe root@<ip-address>:/root/*.ovpn <folder>

Замените <ip-address> на IP-адрес сервера, а <folder> на путь к папке, в которую необходимо сохранить файлы. Папка должна
существовать. Вместо пути к папке можно поставить точку - тогда файлы сохранятся в текущую папку. Например:

    pscp.exe root@123.123.123.123:/root/*.ovpn .

или

    pscp.exe root@123.123.123.123:/root/*.ovpn "c:\vpn"

Файлы, предназначенные для подключения с мобильных устройств, можно отправить по электронной почте, после чего сохранить 
на устройстве.


#### Подключение устройства к VPN.
##### Подключение Windows
##### Подключение MacOS
##### Подключение iOS
##### Подключение Android
##### Подключение Роутера

