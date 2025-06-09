## Administrator-Linux-Lesson-10-Bash
## Скрипты для обработки log-файла

Создание скриптов для анализа log-файла “access-4560-644067.log”.

Задание 1. Получить список IP адресов (Top 10 с наибольшим кол-вом запросов) с указанием кол-ва запросов.

Скрипт 1:

#!/bin/bash

#Путь к файлу логов

LOG_FILE="/home/badger/access-4560-644067.log"

#Проверяем, существует ли файл логов

if [ ! -f "$LOG_FILE" ]; 

then

echo "Ошибка: файл $LOG_FILE не найден!" >&2
    
exit 1
    
fi

#Анализируем логи и сохраняем топ-10 IP в файл loganalysis1

echo "Топ-10 IP-адресов с наибольшим количеством запросов:" > loganalysis1

echo "-----------------------------------------------" >> loganalysis1

awk '{print $1}' "$LOG_FILE" | sort | uniq -c | sort -nr | head -n 10 >> loganalysis1

echo "Анализ завершен. Результаты сохранены в файл loganalysis1."

Результат выполнения скрипта:

badger@ubuntu:~$ ./script1.sh

Анализ завершен. Результаты сохранены в файл loganalysis1.

badger@ubuntu:~$ cat loganalysis1

Топ-10 IP-адресов с наибольшим количеством запросов:

-----------------------------------------------
     45 93.158.167.130
     39 109.236.252.130
     37 212.57.117.19
     33 188.43.241.106
     31 87.250.233.68
     24 62.75.198.172
     22 148.251.223.21
     20 185.6.8.9
     17 217.118.66.161
     16 95.165.18.146

Задание 2. Получить список запрашиваемых URL (с наибольшим кол-вом запросов) с указанием кол-ва запросов.

Скрипт:

#!/bin/bash

#Путь к входному файлу лога

LOG_FILE="/home/badger/access-4560-644067.log"

#Путь к выходному файлу с результатами

OUTPUT_FILE="/home/badger/loganalysis2"

#Проверяем, существует ли входной файл

if [ ! -f "$LOG_FILE" ]; 

then

echo "Ошибка: файл лога $LOG_FILE не найден!" >&2
    
exit 1
    
fi

#Анализируем лог и получаем топ-10 URL

echo "Топ-10 самых запрашиваемых URL:" > "$OUTPUT_FILE"

awk '{print $7}' "$LOG_FILE" | sort | uniq -c | sort -nr | head -n 10 >> "$OUTPUT_FILE"

echo "Анализ завершен. Результаты сохранены в $OUTPUT_FILE"

Результат вывода:

badger@ubuntu:~$ ./script2.sh

Анализ завершен. Результаты сохранены в /home/badger/loganalysis2

badger@ubuntu:~$ cat ./loganalysis2

Топ-10 самых запрашиваемых URL:

    157 /
    120 /wp-login.php
     57 /xmlrpc.php
     26 /robots.txt
     12 /favicon.ico
     11 400
      9 /wp-includes/js/wp-embed.min.js?ver=5.0.4
      7 /wp-admin/admin-post.php?page=301bulkoptions
      7 /1
      6 /wp-content/uploads/2016/10/robo5.jpg
