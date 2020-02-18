# Установка ноды делегата

Автор: [@vik](https://golos.io/@vik)

![](https://imgp.golos.io/0x0/https://s26.postimg.org/6fgrgq2cn/vik.png)

**В последнее время ко мне часто обращались с вопросами по установке делегатских нод и давно нужно было написать инструкцию, которую каждый мог бы получить в виде ссылки.**  
На голосе можно найти старые мануалы, но практически нет информации об активации и настройке режима паблик-api ноды. Я постараюсь подробно описать настройку.

Публичные ноды важная составляющая экосистемы блокчейна, при малом их количестве и нестабильной работе, многие начинают использовать основную ноду ws.golos.io увеличивая нагрузку и нарушая работу клиента golos.io  
Об этом я уже писал ранее: [**Этика ботоводства на голосе и экономия ресурса официальной паблик ноды**](https://golos.io/ru--golos/@vik/etika-botovodstva-na-golose-i-ekonomiya-resursa-pablik-nod-robot-delegat-za-kotorogo-ne-nuzhno-golosovat)

**Установка ноды Делегата \(Witness\)**

**Сервер**

Если вы раньше не арендовали сервера, это может показаться для вас трудной задачей. Но это только так кажется. На самом деле существует масса сервисов и реселлеров, которые принимают оплату в криптовалюте, вам не составит труда оплатить VPS/VDS. А если вы привыкли платить картой - в вашем распоряжении "океан" возможностей. Например Digital Ocean

Запомните, вам нужен сервер с ROOT доступом и с минимум 8 GB \(Если только для тестов, иначе нужно хотя бы 12 GB\) RAM и минимум 40 GB на диске. Желательно SSD диск. При заказе сервера вам будет предложено выбрать предустановленную OS - выбирайте Ubuntu 16.04 и выше.

После того как вы получили в свое распоряжение логин и пароль к серверу, можно подключаться к нему терминалом/консолью по SSH и установить все необходимое в командной строке:

`sudo apt-get update`

`sudo apt-get upgrade`

Если обновления не загружаются из-за ipv6, просто введите команду ниже и повторите снова  
`echo 'Acquire::ForceIPv4 "true";' | tee /etc/apt/apt.conf.d/99force-ipv4`

**Ниже я не использую Docker, поскольку он потребляет лишние ресурсы, к тому же чистая установка будет нагляднее для первого раза**

**Установка зависимостей**

```text
sudo apt-get -y upgrade && sudo apt-get -y install git cmake g++ python-dev autotools-dev libicu-dev build-essential libbz2-dev libboost-all-dev libssl-dev libncurses5-dev doxygen libreadline-dev dh-autoreconf screen
```

**Установка ноды**  
В строке ниже есть фрагмент `tags/v0.16.4`- это версия ноды. Если вам попадется эта инструкция позднее, вам следует проверить, какую актуальную версию блокчейна поддерживает большинство делегатов здесь: [http://golosd.com/witnesses](http://golosd.com/witnesses) и заменить версию в строке ниже на актуальную.

\(Папка с ПО блокчейна голоса установится в ту директорию, в которой вы введете команду ниже\)

```text
git clone https://github.com/GolosChain/golos && cd golos && git checkout tags/v0.16.4 && git submodule update --init --recursive && cmake -DCMAKE_BUILD_TYPE=Release . && make -j$(nproc)
```

⏰ После ввода команды нужно будет подождать несколько минут инсталяции.

После я рекомендую использовать `screen`- это своего рода окна в консоли, между которыми вы можете переключаться не останавливая выполняемые скрипты в них.

Например создайте и перейдите в окно D \(Делегат\)  
`screen -S D`\(Вместо D можно использовать любое слово, это просто id окна\)

> Чтобы выйти из него, нажмите `ctrl`+`A`+`D`  
> Чтобы вернуться снова `screen -x D`

Теперь вы в отдельном окне и можете перейти в папку `golos/programs/golosd/`

![](https://images.golos.io/DQmbRqx6eBDSDRahGD1uKw3k3yfNhzVF7e2uwngkRpp7fJC/image.png)

И ввести там`./golosd`

Это будет первый запуск, во время которого будет показана ошибка, но будут созданы необходимые папки с настройками.  
Вы увидите вывод консоли вида  
![&#x41C;&#x430;&#x441;&#x442;&#x435;&#x440; &#x43D;&#x43E;&#x434;&#x430; GOLOS](https://images.golos.io/DQmSARL2Msbp2Vr8YAb5maGJ4MAXP2tdAQifuXUguk1Mn63/1.PNG)

Файл конфига создан, теперь нужно его найти и отредактировать  
![&#x41C;&#x430;&#x441;&#x442;&#x435;&#x440; &#x43D;&#x43E;&#x434;&#x430; GOLOS](https://images.golos.io/DQmaxLcS4G6ZcD3BP2RtGBxmWGYRV5KjzyHR9LqUyyCrsvy/path.PNG)

Находим его по пути `golos/programs/golosd/witness_node_data_dir/config.ini`

Теперь нам нужно добавить адреса сид-нод для связи с сетью голоса и локальные адреса с доступом по RPC, WS, curl и т.д. для работы с блокчейном

![](https://images.golos.io/DQmQvUUUE5qgdhrPk9V6q6w3Q75jMxpFrXQvxnxjhQcB12T/image.png)

Актуальный список сид-нод можно будет взять тут  
[https://github.com/GolosChain/golos/blob/master/documentation/seednodes](https://github.com/GolosChain/golos/blob/master/documentation/seednodes)

```text
seed-node = 5.9.18.213:4243
seed-node = 52.32.75.69:4243
seed-node = 52.57.156.202:4243
seed-node = 88.99.13.48:4243
seed-node = golos-seed.arcange.eu:4243
seed-node = golos-seed.esteem.ws:4243
seed-node = golosnode.com:4243
seed-node = 138.68.101.115:4243
seed-node = golos.imcoins.org:2001
seed-node = 178.62.224.148:4242
```

RPC адреса

```text
rpc-endpoint =  127.0.0.1:9090
rpc-http-endpoint = 127.0.0.1:9091 
rpc-http-allowip = 127.0.0.1
```

Теперь осталось добавить еще 2 необходимых параметра **witness** и **private-key**

В поле witness добавьте свой логин делегата в кавычках, например`"vik"`  
А в поле **private-key** нужно вставлять **\(!\) приватный ключ подписи** без кавычек, например:

`5FfMuvf3SQQDKhN83zboHYW5FZA1ShqqD2NaaLqo`

Но вы еще не знаете свой приватный ключ подписи. Обычно я временно вставляю туда активный \(это неправильно\) а после синхронизации беру в кошельке ключ подписи. Но сегодня мы сделаем по-другому.  
Как узнать свой приватный **signing key** я напишу ниже.

Вместе с нодой **golosd** мы установили также **cli wallet** \(кошелек для голоса с командной строкой\)  
Обычно мы подключаем кошелек к своей ноде, но поскольку мы ее еще не запустили и она не синхронизировала блокчейн - к ней подключиться нельзя. Мы временно подключимся к публичной ноде.

Вспомним о наших "окнах" screen и создадим новое W \(wallet\):  
`screen -S W`

Перейдем в папку cli\_wallet по пути:  
`golos/programs/cli_wallet/`

И запустим кошелек командой  
`./cli_wallet --server-rpc-endpoint="wss://ws.golos.io"`

Параметр`--server-rpc-endpoint="wss://ws.golos.io"`позволил нам подключиться к рабочей ноде голоса. Напомню, это временно, так как наша нода еще не запущена и на скрине ниже локальный адрес.

После ввода команды вы увидите строку с приглашением`new>>>`  
Вам нужно зашифровать локальный кошелек на сервере придуманным паролем, введите команду после new&gt;&gt;&gt;:

`set_password ВАШ-ПрИдУмАнЫй-ПАРОЛЬ`\*

![&#x41C;&#x430;&#x441;&#x442;&#x435;&#x440; &#x43D;&#x43E;&#x434;&#x430; GOLOS](https://images.golos.io/DQmcSE1UmpseweLKY9g7aHmZUVm87rmcXzZdwSRVfLPANp4/4.PNG)

Вы увидите в ответ locked \(заблокирован\)  
Теперь разблокируйте кошелек командой

`unlock ВАШ-ПрИдУмАнЫй-ПАРОЛЬ`

И увидите приглашение unlocked&gt;&gt;&gt;

Теперь вам нужно импортировать в кошелек свой активный ключ с голоса.  
Его можно взять на вкладке разрешений в кошельке на сайте. Разумеется, это должен быть приватный активный ключ, который показывается после авторизации, он как и постинг начинается на 5.

![](https://images.golos.io/DQmUF9LF2NqnTYnkt477juXDv8GjLGrjEpDCD61gJtoidt6/image.png)

Теперь импортируйте свой ключ командой:

`import_key 5XNqnTYnkt477juXDv8GjLGrjEpDCD61gJtoidt6huy`

Далее введите команду  
`suggest_brain_key`

Вы получите ответ вида:

![](https://images.golos.io/DQmcj7Dfp2djRdCnpxTym5D1PQQThKohfu1CNh2vfMFebiv/image.png)

Надежно сохраните эти ключи \(мы еще вернемся к ним\).  
А ключ, который `wif_priv_key` вставьте в config.ini - это именно тот ключ, с помощью которого ваша делегатская нода будет подписывать блоки.

Вернемcя к ноде `screen -x D`

Настало время запустить синхронизацию, находясь в папке golosd вводим

`./golosd`  
Если все верно, вы увидите вот такой вывод в консоли

![&#x41C;&#x430;&#x441;&#x442;&#x435;&#x440; &#x43D;&#x43E;&#x434;&#x430; GOLOS](https://images.golos.io/DQmW42QhFfMuvf3SQQDKhN83zboHYW5FZA1ShqqD2NaaLqo/2.PNG)

Красные строки на этом этапе не ошибки, а скорее важные заметки. Если процесс синхронизации блоков идет - все хорошо.

⏰ ⏰ ⏰ Запаситесь терпением, это достаточно длительный процесс, на быстрых серверах это займет около часа, на средних - может занять много часов. Можно расслабиться и сходить покакать.

Если взять с собой ноутбук, можно всплакнуть наблюдая принятие прошлых хардфорков

![&#x41C;&#x430;&#x441;&#x442;&#x435;&#x440; &#x43D;&#x43E;&#x434;&#x430; GOLOS](https://images.golos.io/DQmXxSot6UDMmVyQwUPXGoFqtFKg8L8pFTB4dNQ7My4cWde/3.PNG)

Когда в строках \_block изменится на \_transactions и вы будете видеть, что блоки идут последовательно с интервалом в 3 секунды, можно считать, что нода синхронизирована. Если мы будем вносить правки в конфиг, а мы будем, то потом ее нужно будет перезапустить. Нажав `ctrl`+`c` можно остановить ноду. А введя команду `./golosd` в папке `golosd` - восстановить.

![&#x41C;&#x430;&#x441;&#x442;&#x435;&#x440; &#x43D;&#x43E;&#x434;&#x430; GOLOS](https://images.golos.io/DQmdxSzpFd8NMmF4TeDoGe48aKEJdt9jAJb9WWYzBJeShmW/5.PNG)

Пока нода работает, вам нужно объявить себя активным делегатом.  
Выйдем с окна D командой`CTRL+A+D`и перейдем к кошельку`screen -x W`

Теперь нам логично подключиться уже к своей локальной ноде \(помните, мы работали раньше с публичной\)  
Для этого выйдем с текущей сессии кошелька CTRL+C или ENTER и введем команду:

`./cli_wallet --server-rpc-endpoint="ws://localhost:9090"`

Теперь мы подключились к блокчейну локально, через свою ноду.

Вернемя к нашим ключам  
![](https://images.golos.io/DQmcj7Dfp2djRdCnpxTym5D1PQQThKohfu1CNh2vfMFebiv/image.png)

Теперь нам нужен ПУБЛИЧНЫЙ ключ `pub_key`  
Cформируйте строку для команды объявления себя делегатом:

```text
update_witness "vik" "https://golos.io/x/@vik/delegat" GLS8LPBzkzf6ap9gby1mVLSsXnBhC2sp8iQd8mKoMS2ChbiMAhWHf 
{"account_creation_fee":"3.000 GOLOS", "maximum_block_size":65536, "sbd_interest_rate":1000} true
```

Где vik - ваш логин, ссылка - это ваш пост о делегатстве, тот самый публичный ключ \(не вздумайте приватный, эта операция будет публичной и ее увидят все\)  
Остальные параметры можете оставить как в шаблоне выше.

Попробуйте ввести эту команду, если в ответ вы не получите ошибок, можно проверить в golosd.com/@имя и убедиться, что вы смогли отправить транзакцию через свою ноду.

После вы можете опубликовать котировку GOLOS/GBG  
Сделать это можно так:

`publish_feed "vik" { "base":"2.362 GBG", "quote":"1.000 GOLOS"} true`

Позднее вам лучше автоматизировать эту операцию, так как это обязанность делегата, публиковать котировки GOLOS к GBG

Теперь вам нужно получить голоса за свою ноду. Можете проголосовать за себя сами и попросить друзей, чтобы ваша нода хоть немного поднялась в рейтинге и подписался первый блок.

Проголосовать можно здесь  
golos.io/~witnesses или командой в кошельке

`vote_for_witness КТО КОГО true true`

Если вы получили малый вес голосов, ждать первого блока можно достаточно долго.  
Пока вы его не подпишите, выглядеть ваша нода на[http://golosd.com/witnesses](http://golosd.com/witnesses)будет так:  
![](https://images.golos.io/DQmWJKw6QP3PBa32Ha9WbzqcwsEEua22Lr8psW4nA9sqQbQ/image.png)

Если блок не появится или появится пропущенный блок - вам будет нужно пересмотреть настройки и найти ошибку.

Инструкция ниже позволит сделать вашу ноду доступной для использования другим пользователям. Это подразумевает более мощный сервер.

**Делаем ноду публичной и безопасной для всех \(NGINX+TLS/WSS шифрование\)**

У голоса есть публичная нода wss://ws.golos.io к которой подключается множество разработчиков и мешают работе сети создавая узкое место. Вы можете "разгрузить" официальную ноду предложив использовать вашу, для этого вам нужно сделать ее так же доступной для всех.  
Вы можете купить или зарегистрировать бесплатный домен или вовсе использовать IP адрес сервера, просто прописав его как RPC в конфиге. Но если вы намерены держать нагрузку и поддерживать постоянную работу - следует использовать более гибкий инструмент.

Чтобы не "нагружать" вас лишним раньше времени ниже я приведу базовую настройку nginx без rate limit и анализа трафика \(В будущем я опубликую как обезопасить сервер от атак\)

**Установка NGINX**

Как и ранее можно использовать screen и переключиться на отдельное окно и начать установку:

`sudo apt-get update`  
`sudo apt-get install nginx`

Теперь создайте в удобном для себя месте на сервере произвольный файл с расширением .conf, например `golosnode.conf`

Далее добавим путь к этому файлу конфигурации в конфиге nginx  
Открываем в редакторе главный конфиг:

`nano /etc/nginx/nginx.conf`

Ищем там строки с include  
И добавляем после include путь/к/golosnode.conf  
Например:

`include /home/nginx/golosnode.conf;`

Далее откройте этот файл и добавьте nginx правила:

```text
upstream websockets {
  server 127.0.0.1:9090;
  server 127.0.0.1:9091;
}

server {
    listen 80; 
        server_name ВАШ_ДОМЕН_ИЛИ_IP;
    root /home/html/;
    keepalive_timeout 65;
    keepalive_requests 100000;
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;

    location ~ ^(/|/api) {

        limit_req zone=ws burst=5;
        access_log off;
        proxy_pass http://websockets;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_next_upstream error timeout invalid_header http_500;
        proxy_connect_timeout 2;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }

}
```

После сохранения перезапустите nginx и проверьте статус

`systemctl restart nginx`  
`systemctl status nginx.service`

При отсутствии ошибок вы увидите примерно такой вывод в консоли

![&#x41C;&#x430;&#x441;&#x442;&#x435;&#x440; &#x43D;&#x43E;&#x434;&#x430; GOLOS](https://images.golos.io/DQmX2HmJWFxyPicw4J6sxr9ziEFSZbtuUKXtJiUykLaT738/6.PNG)

К редакциям конфигов мы еще вернемся, теперь установим TLS сертификат для шифрования нашего трафика.

**🔐 Шифрование и безопасность коннекта TLS/SSL/WSS**

Установить сертификат стало очень просто с сервисом-ботом `CertBOT`  
Введите команды ниже:

```text
sudo apt-get update
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install python-certbot-nginx 
sudo certbot --nginx
```

Вы увидите диалоговое окно, где вам будет предложено ввести свой email и указать домен для которого вы собираетесь установить сертификаты  
![&#x41C;&#x430;&#x441;&#x442;&#x435;&#x440; &#x43D;&#x43E;&#x434;&#x430; GOLOS](https://images.golos.io/DQmd95BHuxMJDWER1jPM9kpbkKzz8pLcLDA2VESksMW3K4R/7.PNG)

Так же вам будет предложено автоматически добавить редирект с http на https , но так как нам нужен редирект с ws на wss - можно эту опцию не выбирать.  
![&#x41C;&#x430;&#x441;&#x442;&#x435;&#x440; &#x43D;&#x43E;&#x434;&#x430; GOLOS](https://images.golos.io/DQmRThxRxDPcXazwKx7G1yYreCtL7gUv2ZtJfYg3XVguhKe/8.PNG)

После всего этого, certbot самостоятельно добавит настройки в ваш `golosnode.conf;`, но вам лучше всё перепроверить:

```text
server {
    listen 80; # УДАЛИТЬ!
        server_name ВАШ_ДОМЕН_ИЛИ_IP;
    root /home/html/;

        listen 443 ssl; # ДОЛЖНО БЫТЬ ДОБАВЛЕНО
    ssl_certificate /etc/letsencrypt/live/api.golos.cf/fullchain.pem; # ДОЛЖНО БЫТЬ ДОБАВЛЕНО
    ssl_certificate_key /etc/letsencrypt/live/api.golos.cf/privkey.pem; # ДОЛЖНО БЫТЬ ДОБАВЛЕНО
    include /etc/letsencrypt/options-ssl-nginx.conf; # ДОЛЖНО БЫТЬ ДОБАВЛЕНО
*** остальные настройки такие же ***
```

Я оставил полный конфиг тут  
[https://github.com/vikxx/server](https://github.com/vikxx/server)  
Он работает так:  
Доступ по вебсокетам по адресу`wss://api.golos.cf`  
Доступ к html страничкам из папки html по адресу [https://golos.cf](https://golos.cf/)  
Используя его как шаблон, вы сможете настроить правильные пути для своего домена.

После правки nginx настроек его следует перезапускать  
`systemctl restart nginx` - перезапуск  
`systemctl status nginx.service` - проверяем нет ли ошибок

**Теперь вернемся к ноде голоса, к файлу config.ini**

Нам нужно указать список плагинов для активации, а так же API запросы, которые мы разрешим для пользователей

```text
enable-plugin = account_by_key
enable-plugin = witness
enable-plugin = account_history
enable-plugin = follow
enable-plugin = market_history
enable-plugin = tags
public-api = database_api login_api market_history_api tags_api follow_api network_broadcast_api use_network_node_api
```

Так же найдите строку`server-pem`и добавьте путь к сертификату, его можно увидеть в конфиге к nginx

После правки config.ini можно перезапустить ноду `ctr+c` стоп, `./golosd` запуск.

Чтобы понять корректно ли работает ваша нода, можете попробовать подключить к ней cli\_wallet указав публичный путь, а не локальный.

**Позднее я добавлю инструкции по настройке фаервола и алгоритмов защиты вашей ноды от злоупотребления.**

Пример работы паблик ноды можно увидеть тут [https://golos.cf](https://golos.cf/)  
Сайт подключен к api.golos.cf

![](https://images.golos.io/DQmTwTLB1yP5mYQTjen2pjZmRp5CGQGCE6feSFd3rMa2GJN/image.png)

Так же вы можете подключить к `wss://api.golos.cf` собственные скрипты.

Конфигурация сервера описана мной [ранее в блоге](https://golos.io/ru--golos/@vik/publichnaya-api-noda-dlya-razrabotchikov-golosa-golos-cf-wss-api-golos-cf)

> По материалам [статьи](https://golos.io/ru--golos/@vik/aktualnaya-instrukciya-po-ustanove-delegatskoi-nody-golosd-sozdanie-obshedostupnoi-pablik-api-nody-s-tls-wss-shifrovaniem-na).
>
> Автор [@vik](https://golos.io/@vik)

