#Асинхронный и синхронный API

## Типы
- request-based
- message-based

## Схемы - взять из презентации

## Расчет SLA
- у message-base надежность и стабильность выше

## Определения
**Синхронное взаимодействие** - это такое взаимодействие при котором семантика сервиса A такова, 
что он не может продолжить выполнения запроса, если не получил ответа от B

**Асинхронное взаимодействие** - это такое взаимодействие при котором сервис A в соответствии 
с семантикой предметной области может продолжить выполннение, не дожидаясь ответа от B.

## Паттерны
### Message bus pattern
Особенности такого решения:
- Service discovery - сервисам не нужно знать где и кто
находится, достаточно просто бросить сообщение в
очередь
- Выше доступность – если сервис упал, то он просто не
берет запросы из очереди
- Ниже вероятность каскадных падений
- Горизонтальное масштабирование
- На стороне message bus можно делать различные
преобразования, сохранение, логирование, проверки
- Увеличивает эффективное использование ресурсов
сервисов, написанных на языках не поддерживающих
асинхронное программирование  
  

- Latency увеличивается
- Debug становится сложнее
- Single Point Of Failure в виде брокера сообщений

### Async update/sync read (CQS)
**SCQ - Command Query Segregation** - принцип в соответствии с которым все запросы следует разделить на:
- запросы чтения (query), которые возвращают результат и не имеют побочных эффектов
- на изменения (command), которые ничего не возвращают

Иными словами - не должно быть изменений которые бы возвращали какие-то значения.  

Если организовать взаимодействие в виде команд и запросов, то тогда
1) Использовать для запросов - синхронное взаимодействие
2) Для команд - асинхронное взаимодействие

## Оркестрация и хореография
**Оркестрация** - один сервис координирует весь процесс.
**Хореография** - сервисы координируются между собой за счет асинхронного взаимодействия и событийной модели.

###Оркестрация vs хореография
- **Оркестрация** дает четкое понимание, что происходит и как выглядит бизнес процесс
- **Оркестрация** может излишне «связать» сервисы друг с другом
- **Хореография** не всегда дает четкое понимание, что и когда происходит.
- **Хореография** «развязывает» сервисы и им нет необходимости знать друг про друга
  
## IDL
**IDL** - interface definition language.
OpenAPI - для синхронных   
AsyncAPI - для асинхронных
GraphSchema - для GraphQL


