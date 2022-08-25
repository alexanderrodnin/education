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

