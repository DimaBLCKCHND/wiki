# Типы и роли нод

Нода, это сердце блокчейна, программное обеспечение, которое обрабатывает блок за блоком, исполняет все операции и хранит состояние системы. Владелец настраивает ноду, выбирает используемые плагины. От включенных плагинов и их настроек зависит то, какие возможности может предоставлять нода.

## Witness node \(нода делегата\)

За формирование блоков отвечают делегаты, поэтому важнейшими нодами являются именно они. С помощью них обмениваются данными с другими нодами, собирают транзакции и когда подходит очередь делегата \(владельца сервера\) сформировать блок — происходит подпись собранного блока и его трансляция другим узлам. Блок должен быть подписан и доставлен за 3 секунды, выделенные на это делегату. Пропуск блока приводит к задержке выполнения транзакций.

Зачастую делегаты держат две ноды, основную и запасную \(резерв\). Часто они находятся в разных дата-центрах и не зависят друг от друга. Если с основной нодой происходит неполадка, то делегат меняет ключ подписи блоков на резервный и формированием блоков будет заниматься запасная нода.

## Seed node \(сид-нода\)

Основа для функционирования любой блокчейн системы — peer-to-peer \(p2p\) соединение и обмен данными. Сид-ноды отличаются тем, что выполняют важную роль — принимают и раздают блоки, разгружая таким образом пропускную способность всей сети и снижая отклик для близких территориально подключений. Нет экономической выгоды держать сид-ноду, так как сервер стоит денег, но не приносит своему владельцу ничего. Поэтому большинство сид-нод запускают делегаты \(witnesses\), если они могут позволить себе это финансово.

## API node

Часть плагинов занимаются предоставляем API для разработчиков и их пользователей. Такие ноды часто называют полными, если у них включены все доступные плагины и хранят историю с первого блока. Так как плагины дают доступ не только к состоянию системы, но и формируют свои структуры данных, у них повышенные требования к ресурсам сервера \(особенно к оперативной памяти, так как нода хранит базу данных ChainBase в RAM\). Именно через API ноды происходят запросы на актуальную информацию, статус аккаунтов, историю операций или заявки в комитете.

Примеры таких плагинов:

* **network\_broadcast\_api** — отправка транзакции в сеть;
* **database\_api** — основной плагин предоставляющий доступ к состоянию системы, получение данных об аккаунтах, получение информации о блоке, получение параметров сети;
* **account\_history** — получение списка операций связанных с аккаунтом;
* **worker\_api** — получение списка заявок, информации о заявке, получение списка голосов по заявке;
* **operation\_history** — получение информации об операциях в блоке;
* **witness\_api** — получение списка делегатов, очереди делегатов, информации о конкретном делегате и его голосуемых параметрах сети.

API ноды могут быть как приватными \(когда в настройках указаны параметры доступа по логину и паролю\), так и публичными \(когда обращение к API доступно всем и публично известен адрес ноды\). Часто публичные API ноды называют просто публичными нодами. Подробнее читайте в разделе [Плагины и их API](../developers/basics/plugins-api.md).

