 ## Код возврата
 ```bash
# получить код возврата
# $? - это переменна в которой хранится последний код возврата 
# Код возрават - может быть == 0 - если программа отработала правильно и НЕ!0 если отработала неверно
echo $?

# нарпимер
ls -l
echo $? # вернет 0

ls -l asfdjfdhasjfksd
echo $? # вернет 2
 ```
## TRUE/FALSE - в bash
true == 0   
false != 0

## Логическое И - &&
 ```bash
# если мы хотим чтобы одна команда выполнилась после корректного выполнения предыдущей - 
# следует исопльзовать логическое И
ls -l && echo ok
 ```

## Логическое ИЛИ - ||
 ```bash
# если мы хотим чтобы одна команда выполнилась после НЕ корректного выполнения предыдущей - 
# следует исопльзовать логическое И
ls -l афыаывфа && echo ok
 ```

## Склейка команд
 ```bash
# если мы хотим чтобы одна команда выполнилась после не зависимо от исхода предыдущей - 
# следует исопльзовать ;
ls -l афыаывфа ; echo ok
 ```

## Цикл for
```bash 
for i in {2015..2022}; do echo $i; done
```

## Скрипт
Это набор команд для того или иного интерпретатора
```bash
# Информация о дефолтовой оболочке хранится в
echo $SHELL
```
для того чтобы указать какой именно интерпретатор использовать первой строкой в .sh файле следует указать
```bash
#!/bin/bash
```


