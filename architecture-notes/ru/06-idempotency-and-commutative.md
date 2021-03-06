# Идемпотентность и коммутативность

## Идемпотентность в API
Идемпотентность операции - можно послать несколько раз один и тот же запрос (сообщение), и состояние это не изменит.  
Вообще retriability - возможность безопасно повторить запрос - очень полезная штука, 
и если есть возможность легко сделать запрос или событие идемпотентным - его стоит таким сделать.

### Вариант решения проблемы - добавление ключа
Одним из вариантов решения может быть - добавление ключа.
Например при совершении покупки:
```text
POST /api/v1/orders
X-Request-Id: qweqwe-ewqwe-asd-ew
{
  “products”: [{“id”: 42, “price”: “2500”}],
  “shipping_to”: “Большая Филевская 14, кв. 88”
}
```
Проблема с ключем - где его хранить на клиенте.
Пример: [тут](https://stripe.com/docs/api/idempotent_requests)

### Еще варианты
- использование оптимистических блокировок
- версионирование, timestamp
- сравнение с состоянием на бэке

Идемпотентность для коллекций
Делаем версионирование коллекции /api/v1/orders/.  
Сервер присылает заголовок ETag с версией коллекции orders.  
Клиент при изменении коллекции заголовок If-Match с версией, которую он знает.
```text
/api/v1/orders/
Etag: 42
```
Сервер проверяет: если If-Match совпадает с версией на сервере, то запрос проходит. Если нет, то отвечает ошибкой.
```text
POST /api/v1/orders/
If-Match: 42
```
Ответы следует делать с учетом кодов HTTP - например HTTP 429 - To many requests

### Идемпотентность ссылки
[Yandex habr](https://habr.com/ru/company/yandex/blog/442762/)


### Idempotent reciever
Из продюсера
- отправлять сообщения вместе с ключом идемпотентности (msg_id)  

В консьюмере:  
- Завести табличку с отработанными сообщениями (processed_message)
- Коммиты в табличку с данными и processed_table происходят транзакционно
- Если сообщение уже было, то ничего не делать


## Коммутативные сообщения
К сожалению, порядок сообщений также может портиться.  
Даже в случае, если сам брокер сообщений предоставляет гарантии порядка внутри себя, все-равно у нас есть:  
- Конкурентные продюсеры:  
От нескольких продюсеров сообщения могут приходить в разном порядоке
- Конкурентные консьюмеры:  
Консьюмеры могут обрабатывать сообщения в разном порядке.

### Вариант решения: Polling publisher
Если у нас есть transactional outbox, мы можем тогда в »безопасном» режиме написать
отдельный сервис, который забирает события из БД и помечает их как отправленные. И
отправляет их гарантированно в том порядке, в котором надо.  
Для увеличения нагрузки, можем запустить несколько инстансов сервиса (асинхронной
джобы), каждый из которых забирает свой набор объектов. Обязательно, чтобы все
события для одного объекта, попали только к одному инстансу.
```sql
select * from outbox order by ... asc
begin
    delete from outbox where id in (...)
end
```

### Вариант решения: Sharded consumers
Для того, чтобы убрать конкурентность со стороны консьюмеров – делаем тоже самое, каждый получатель присасывается к своему шарду. 
Шарды сделаны таким образом, чтобы события (сообщения) про один объект были в одном шарде

### Вариант решения: Паттерн Compare-and-set
Для решения проблемы идемпотентности и коммутативности, можем скрестить события, которые несут изменения с событиями, которые несут состояния.  
В этом случае, продюсер, прежде чем применять событие сравнивает текущеесостояние и если оно совпадает, 
то применяет событие, в ином случае возвращает событие назад в очередь.  
Пример:
```json
{
    "oldState": "BLOCKED",
    "newStage": "ACTIVE"
}
```

### Вариант решения: Версионирование
Когда крайне важен порядок сообщений можно использовать более сложную схему, похожу на репликацию данных из БД.
Во все события (сообщения) прокидываем либо sequence_id, либо переход из старой версии в новую.  
Пример:
```json
{
    "data": "data",
    "oldVersion": 3,
    "newVersion": 4    
}
```

### Вариант решения: Transaction log miner.
Если честно - то не понятно как он решает проблему.  
Принцип - чтение лога базы.

### Links
[Rambler](https://www.youtube.com/watch?v=oByOmhOmOh4)
[Mail](https://habr.com/ru/company/mailru/blog/219015/)

## Синхронизация данных
Синхронизация - это история про обеспечение консистентности данных.


### Links
[Badoo habr](http://www.highload.ru/2017/abstracts/2930.html)
