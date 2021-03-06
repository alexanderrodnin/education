# Транзакции, MVCC, ACID

## Транзакция
Транзакция - Последовательность операций, рассматривающихся базой данной как атомарное действие и завершающаяся подтверждением изменений (commit) либо откатом изменений (rollback).
  
Проблема, которую решают транзакции - **множественная параллельная нагрузка**.  
Как обеспечить параллельную работу множества сессий чтобы они могли:
- модифицировать данные
- не мешать друг другу (ни при записи ни при чтении)
- обеспечить целостность данных
- обеспечить надежность данных

*Способы реализации ACID*
- Aries 
- MVCC
  [see](05-cap-theorem.md)

## Vacuum и AutoVacuum
Удаление информации о незакоммиченных транзакциях и проч неиспользуемых строк.  
AutoVacuum - должен быть включен - это правило хорошего тона.

## Уровни изоляции транзакции
Стандарт SQL допускает четыре уровня изоляции, которые определяются в терминах аномалий, которые допускаются при конкурентном выполнении транзакций на этом уровне:
- «Грязное» чтение (dirty read). Транзакция T1 может читать строки измененные, но еще не
зафиксированные, транзакцией T2. Отмена изменений (ROLLBACK) в T2 приведет к тому,
что T1 прочитает данные, которых никогда не существовало. (В ПОСТРЕСС ЭТОГО НЕТ)
- Неповторяющееся чтение (non-repeatable read). После того, как транзакция T1 прочитала
строку, транзакция T2 изменила или удалила эту строку и зафиксировала изменения
(COMMIT). При повторном чтении этой же строки транзакция T1 видит, что строка изменена
или удалена.
- Фантомное чтение (phantom read). Транзакция T1 прочитала набор строк по некоторому
условию. Затем транзакция T2 добавила строки, также удовлетворяющие этому условию.
Если транзакция T1 повторит запрос, она получит другую выборку строк.
- Аномалия сериализации - Результат успешной фиксации группы транзакций оказывается
несогласованным при всевозможных вариантах исполнения этих транзакций по очереди. 
