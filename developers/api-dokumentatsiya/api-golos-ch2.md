# API part 2

Автор: [@asuleymanov](https://golos.io/@asuleymanov)

В прошлой [статье](https://golos.io/ru--otkrytyij-kod/@asuleymanov/opisanie-api-golos-chast-1) было описано много команд. Если быть точным 54. В этой статье я постараюсь описать оставшиеся команды из раздела Database\_API. А также приоткрою завесу тайны на некоторые команды из прошлого выпуска.  
В общем в данный выпуск вошли три группы команд. Условно разделю их на три типа:

1. **Невыясненные**
   * сюда вошли команды результаты вызова которых не ясны. В основном это связано с тем, что так и не нашлось вразумительное объяснение, какой параметр надо передавать.
2. **Команды из прошлого выпуска**
   * сюда вошли 4 команды из прошлого выпуска, так как удалось разобраться что за параметр необходимо передавать.
3. **Неразобранные**
   * сюда вошли команды описание которых не вошло в прошлый выпуск.

## Невыясненные

_**set\_subscribe\_callback**_  
Параметры:`"method":"set_subscribe_callback", "params":[["cb","clearfilter"]], "id":0`  
Описание: Не выяснено.

_**set\_pending\_transaction\_callback**_  
Параметры:`"method":"set_pending_transaction_callback", "params":["cb"], "id":1`  
Описание: Не выяснено.

_**set\_block\_applied\_callback**_  
Параметры:`"method":"set_block_applied_callback", "params":["cb"], "id":2`  
Описание: Не выяснено.

_**cancel\_all\_subscriptions**_  
Параметры:`"method":"cancel_all_subscriptions", "params":["cb"], "id":3`  
Описание: Не выяснено.

## Команды из прошлого выпуска

_**get\_ops\_in\_block**_

Параметры:`"method":"get_ops_in_block", "params":[block_num,only_virtual], "id":21`

Описание: Отображает операции которые были в указанном блоке. При этом если параметр only\_virtual выставлен в значение true то отображаются только виртуальные операции\(что это выяснить так и не удалось\)

> [@ropox](https://golos.io/@ropox)  
> Как я понимаю, "виртуальные операции" операции сделанные самим блокчейном. author\_reward, curation\_reward. Их не увидеть по команде get\_block, только вот так. get\_ops\_in\_block или get\_account\_history. К примеру блок 6754128.

_**get\_transaction\_hex**_

Параметры:`"method":"get_transaction_hex", "params":["trx"], "id":53`

Описание: Отображает HEX строку\(что именно это такое не ясно\). В параметр передается подписанная транзакция.

_**get\_potential\_signatures**_

Параметры:`"method":"get_potential_signatures", "params":["trx"], "id":56`

Описание: Отображает потенциальный ключ для данной транзакции. В параметр передается подписанная транзакция.

_**verify\_authority**_

Параметры:`"method":"verify_authority", "params":["trx"], "id":57`

Описание: Отвечает TRUE если транзакция подписана правильно. В параметр передается подписанная транзакция.

**P.S. Описание транзакции, т.е. её составление я попробую разобрать в следующей статье.**

## Не разобранные

Отступление. Все нижеописанные команды в виде параметр принимают структуру/объект такого вида\(условно назову его _**dsc**_\):

```text
uint32 limit = 0  - количество возвращаемых записей не может быть больше 100. По умолчанию 0 
map[string] select_authors - массив содержит имена авторов 
map[string] select_tags - Список тегов, сообщения без этих тегов фильтруются
map[string] filter_tags - Список тегов, сообщения с этими тегами фильтруются;
uint32 truncate_body = 0 - Количество байтов возвращаемого тела сообщения, 0 для всех, *параметр не обязателен*
string start_author - имя автора с которого начинать искать, *параметр не обязателен*
string start_permlink - ссылка публикации с которой начинать искать, *параметр не обязателен*
string parent_author - имя автора стартовавшего дискуссию, *параметр не обязателен*
string parent_permlink - постоянная ссылка на родительскую дискуссию, *параметр не обязателен*
```

Пояснения. Вначале указан тип параметра, а потом сам параметр.

_**get\_discussions\_by\_trending**_  
Параметры:`"method":"get_discussions_by_trending", "params":[dsc], "id":6`  
Описание: Отображает ограниченное количество публикаций начиная с самой дорогой по вознаграждению.

_**get\_discussions\_by\_trending30**_  
Параметры:`"method":"get_discussions_by_trending30", "params":[dsc], "id":7`  
Описание: Отображает ограниченное количество публикаций по вознаграждению.

_**get\_discussions\_by\_created**_  
Параметры:`"method":"get_discussions_by_created", "params":[dsc], "id":8`  
Описание: Отображает ограниченное количество публикаций начиная с самой новой.

_**get\_discussions\_by\_active**_  
Параметры:`"method":"get_discussions_by_active", "params":[dsc], "id":9`  
Описание: Отображает ограниченное количество записей в которых была активность начиная с самой новой.

_**get\_discussions\_by\_cashout**_  
Параметры:`"method":"get_discussions_by_cashout", "params":[dsc], "id":10`  
Описание: Отображает ограниченное количество публикаций, отсортированных по времени выплат.

_**get\_discussions\_by\_payout**_  
Параметры:`"method":"get_discussions_by_payout", "params":[dsc], "id":11`  
Описание: Отображает ограниченное количество публикаций , отсортированных по выплатам.

_**get\_discussions\_by\_votes**_  
Параметры:`"method":"get_discussions_by_votes", "params":[dsc], "id":12`  
Описание: Отображает ограниченное количество публикаций, отсортированных по величине голосов.

_**get\_discussions\_by\_children**_  
Параметры:`"method":"get_discussions_by_children", "params":[dsc], "id":13`  
Описание: Отображает ограниченное количество публикаций, отсортированных по количеству комментариев.

_**get\_discussions\_by\_hot**_  
Параметры:`"method":"get_discussions_by_hot", "params":[dsc], "id":14`  
Описание: Отображает ограниченное количество публикаций, отсортированных по популярности.

_**get\_discussions\_by\_feed**_  
Параметры:`"method":"get_discussions_by_feed", "params":[dsc], "id":15`  
Описание: Отображает ограниченное количество публикаций, из фида конкретного автора

_**get\_discussions\_by\_blog**_  
Параметры:`"method":"get_discussions_by_blog", "params":[dsc], "id":16`  
Описание: Отображает ограниченное количество публикаций, из блога конкретного автора.

_**get\_discussions\_by\_comments**_  
Параметры:`"method":"get_discussions_by_comments", "params":[dsc], "id":17`  
Описание: Отображает ограниченное количество публикаций, из комментариев конкретного автора.

_**get\_discussions\_by\_promoted**_  
Параметры:`"method":"get_discussions_by_feed", "params":[dsc], "id":18`  
Описание: Отображает ограниченное количество публикаций, отсортированных с помощью увеличенной суммы баланса \(К сожалению мне так и не удалось понять что тут имеется ввиду.\)

Историческая справка

* [Статья № 1](https://www.gitbook.com/book/cyberfund/golos/edit#)
  * Начало разбора команд из раздела Database\_Api
* [Статья № 2](https://www.gitbook.com/book/cyberfund/golos/edit#)
  * Окончание разбора команд из раздела Database\_Api
* [Статья № 3](https://www.gitbook.com/book/cyberfund/golos/edit#)
  * Разбор команд из разделов Market\_History\_API и Follow\_API

