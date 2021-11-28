# Оптимизация, профилирование, мониторинг.

## Дашборды
- [Percona](https://www.percona.com/)
- [Grafana](https://grafana.com/)
- [Pgadmin](https://www.pgadmin.org/)

## Производительность
- **pg_stat_activity** - Одна строка для каждого серверного процесса c информацией по текущей активности процесса, такой как состояние и текущий запрос. За подробностями обратитесь к pg_stat_activity.
    - **track_activity_query_size** - 1024 значение по умолчанию.
В современных системах для этого в большинстве случаев недостаточно
    - ```sql
        SELECT now() - query_start as "runtime", usename, datname, wait_event_type, state, query
        FROM pg_stat_activity WHERE now() - query_start > '5 seconds'::interval and state='active'
        ORDER BY runtime DESC;
        ```
        Далее убиваем подобные данные длительные запросы:
        ```sql
        SELECT pg_cancel_backend(procpid); для active
        SELECT pg_terminate_backend(procpid); для idle
        ```
        Повисшие транзакции:
        ```sql
        SELECT pid, xact_start, now() - xact_start AS duration
        FROM pg_stat_activity
        WHERE state LIKE '%transaction%'
        ORDER BY duration DESC;
        ```
- **pg_stat_statements** - представление, которое позволит посмотреть по истории.
    ТОП по загрузке CPU
    ```sql
    ТОП по времени выполнения
    SELECT substring(query, 1, 100) AS short_query, round(total_time::numeric, 2) AS total_time, calls,
           rows, round(total_time::numeric / calls, 2) AS avg_time, round((100 * total_time /
                                                                           sum(total_time::numeric) OVER ())::numeric, 2) AS percentage_cpu
    FROM pg_stat_statements ORDER BY avg_time DESC LIMIT 20; 
    ```
    ТОП по времени выполнения
    ```sql
    SELECT substring(query, 1, 100) AS short_query, round(total_time::numeric, 2) AS total_time, calls,
           rows, round(total_time::numeric / calls, 2) AS avg_time, round((100 * total_time /
                                                                           sum(total_time::numeric) OVER ())::numeric, 2) AS percentage_cpu
    FROM pg_stat_statements ORDER BY avg_time DESC LIMIT 20; 
    ```
- **pg_stat_user_tables**  Самое большое зло – «последовательное чтение» больших таблиц.
    ```sql
    SELECT schemaname, relname, seq_scan, seq_tup_read, seq_tup_read / seq_scan AS avg, idx_scan
    FROM pg_stat_user_tables
    WHERE seq_scan > 0
    ORDER BY seq_tup_read DESC
    LIMIT 25;
    ```  
    Сверху этого запроса и будут таблицы в которых больше всего операций последовательного чтения.
    Они и будут подозрительны для анализа причин отсутствующих индексов. Надо бы ещё посмотреть кэширование этих таблиц по представлению.
  
## Тюнинг настроек.
Основные настройки:
- effective_cache_size - 2/3 RAM
- shared_buffers = RAM/4 
- temp_buffers = 256MB
- work_mem = RAM/32
- maintenance_work_mem = RAM/16

Также существуют готовые конфигураторы:
[Cybertec](http://pgconfigurator.cybertec.at/)
[Leopard](https://pgtune.leopard.in.ua/)


### Настройка дисков
- fsync – данные журнала принудительно сбрасываются на диск с кэша ОС.
- full_page_write – 4КБ ОС и 8КБ Postgres. Могут быть сбои при записи
- synchronous_commit – транзакция завершается только когда данные фактически сброшены на диск
- checkpoint_completion_target – чем ближе к единице тем менее резкими будут скачки I/O при операциях checkpoint
- effective_io_concurrency – по количеству дисков и random_page_cost – отношение рандомного чтения к последовательному. Впрямую на производительность не влияют, но могут существенно влиять на работу оптимизатора.

### Настройка оптимизатора
- join_collapse_limit – сколько перестановок имеет смысл делать для поиска оптимального плана запроса default_statistics_target - число записей просматриваемых при сборе статистики по таблицам. Чем больше тем тяжелее собрать статистику. Статистика нужна, к примеру для определения «плотности» данных.
- online_analyze - включает немедленное обновление статистики
- online_analyze.enable = on
- online_analyze.table_type = "all"
- geqo – включает генетическую оптимизацию запросов 
```text
  enable_bitmapscan = on
  enable_hashagg = on
  enable_hashjoin = on
  enable_indexscan = on
  enable_indexonlyscan = on
  enable_material = on
  enable_mergejoin = on
  enable_nestloop = on
  enable_seqscan = on
  enable_sort = on
  enable_tidscan = on
  ```

## Планы запросов
Самые частые ошибки оптимизатора (90% проблем)
1) Разница в rows – в оценочном и действительным планом – если значения отличаются в разы – это «звоночек»
2) Nested Loops с большим cost или большим rows – вложенными циклами должны соединяться только маленькие (до тысяч строк) таблицы
3) Seq Scan по таблице где rows более нескольких тысяч, при этом есть
   FILTER – в этом случае явно нужно посмотреть на поля в FILTER и
   найти подходящий индекс. Если не нашли – бинго! Одна из проблем
   решена.
   

### Построение плана запроса
[Tatiyants](https://tatiyants.com/pev/)
[Depesz](https://explain.depesz.com/)

### Рекомендации по оптимизации
- делать меньше запросов
- читать меньше данных
- обновлять меньше данных (несколько update insert в одну транзакцию)
- уменьшать обработку на лету
- проанализировать передачу данных:
    - сколько данных было просканировано - сколько отослано
    - сколько было отослано - сколько использовано приложением
- не использовать UNION, когда можно UNION ALL
- использовать в выборке только нужные столбцы select *
- избегать distinct
- при использовании сложных индексов учитывать наличие в запросе первого столбца индекса
- избегать декартова произведения в джойнах
- при использовании нескольких таблиц давать имена столбцам с алиасами таблиц
- используйте читабельный синтаксис SQL
- используйте понятные названия и camelCase
- не используем русский язык в именах полей

## Крайние меры
- снять нагрузку на диск
- явно повлиять на план запроса
- запросы like
- PgMemcached
- Columnstore (выборки из больших таблиц но без update и delete)
- TimescaleDB - плагин - аналог прометеуса

