# Права на файлы и директории

```bash
# показать ids и группы/права
id alexanderrodnin
uid=501(alexanderrodnin) gid=20(staff) groups=20(staff),12(everyone),61(localaccounts),79(_appserverusr),80(admin),81(_appserveradm),98(_lpadmin),703(com.apple.sharepoint.group.3),705(com.apple.sharepoint.group.5),701(com.apple.sharepoint.group.1),704(com.apple.sharepoint.group.4),33(_appstore),100(_lpoperator),204(_developer),250(_analyticsusers),395(com.apple.access_ftp),398(com.apple.access_screensharing),399(com.apple.access_ssh),400(com.apple.access_remote_ae),702(com.apple.sharepoint.group.2)
```
## Типы пользоватей
Пользователи в Linux делятся на реальных пользователей и пользователей от лица которых запускаются программы  
UID до 1000 - это системные/демоны  
UID после 1000 - (в мак после 500) - это обычные пользователи  
UID у root - всегда 0  

## Группы
В линукс пользователь не может существовать без группы.  
Права в линукс поделены на 3 группы: 1) для пользователя 2) для его primary group 3) права для всех остальных.  

У каждого файла в linux как правило есть владелец файла. Как правило это тот кто создал.  
Для файла или каталога - есть для 1) того кто владелец 2) Для тех кто входит в группу владельца и есть 3) права для всех остальных.
Всего есть 3 типа не исключающих прав: на чтение, на запись на выполнения (rwx)
Пример:
```bash
# d(первые 3 rwx - это для пользователя)(вторые 3 rwx - это для группы)(третьи 3 rwx - это для всех остальных)
alexanderrodnin@MacBook-Air-2 dev % ls -l
total 56
drwxr-xr-x  19 alexanderrodnin  staff    608 Jul 26 11:05 


drwxr-xr-x   3 alexanderrodnin  staff     96 Aug  3 17:23 alfa2
drwxr-xr-x   5 alexanderrodnin  staff    160 Jul  4 20:03 composableFi
```

# Файлы в которых хранится информация о пользователях в Linux
```bash
# Пользователи
/etc/passwd
# Группы
/etc/group
# Пароли
/etc/shadow
```
Если необходимо перенести пользователей на другой инстанс - следует перенести эти 3 файла.

```bash
# Изменение прав
chmod 
# Выдать всем пользователям и всем группам все права рекурсивно
chmod -r 777
# Еще пример простановки прав с указанием пользователя, группы пользователя, других
chmod u+rwx,g+rw,o-rwx ./err
```



