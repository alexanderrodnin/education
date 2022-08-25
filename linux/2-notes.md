### Стандартный вывод, поток
```bash
# 1> - записать в поток стандартного вывода в случае положетельного исхода
# 2> - поток стандартного вывода в случае ошибки
alexanderrodnin@MacBook-Air-2 dev % ls -l 1> STDOUT 2>STDERR
alexanderrodnin@MacBook-Air-2 dev % ls | grep STD
# сюда записаны ошибки
STDERR
# сюда записан стандартный вывод
STDOUT
```

### Запись/дописывание в ресурс
```bash
# > - запись в ресурс >> - дописывание в ресунс 
# >> - дописывание в ресунс
alexanderrodnin@MacBook-Air-2 dev % cat test1
1
alexanderrodnin@MacBook-Air-2 dev % echo "2" > test1
alexanderrodnin@MacBook-Air-2 dev % cat test1       
2
alexanderrodnin@MacBook-Air-2 dev % echo "3" >> test1
alexanderrodnin@MacBook-Air-2 dev % cat test1        
2
3
```

### Порядок выполнения
```bash
# выполнение при налиции > с права на лево.
# если в данном примере нет файла - он будет создан
alexanderrodnin@MacBook-Air-2 dev % ls -l > qwe
alexanderrodnin@MacBook-Air-2 dev % cat qwe 
total 48
drwxr-xr-x   8 alexanderrodnin  staff    256 Aug 23 16:52 personal
-rw-r--r--   1 alexanderrodnin  staff      0 Aug 25 16:19 qwe

# при повторном обновлении файл будет пересоздан с нулевым размером
alexanderrodnin@MacBook-Air-2 dev % ls -l > qwe
alexanderrodnin@MacBook-Air-2 dev % cat qwe 
total 48
drwxr-xr-x   8 alexanderrodnin  staff    256 Aug 23 16:52 personal
-rw-r--r--   1 alexanderrodnin  staff      0 Aug 25 16:19 qwe
```

### Перенаправление из стандартного потока вывода в стандартный поток ввода
```bash
# результат из стандартного вывода  ls /etc будет перенаправлен на стандартный вход в less 
ls /etc | less
```

