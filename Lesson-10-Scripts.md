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


Задание 3. Вывести ошибки веб-сервера/приложения.

Скрипт:

#!/bin/bash

#Пути к файлам

LOG_FILE="/home/badger/access-4560-644067.log"

OUTPUT_FILE="/home/badger/loganalysis3"

#Проверяем, существует ли файл лога

if [ ! -f "$LOG_FILE" ]; 
then

 echo "Ошибка: файл лога $LOG_FILE не найден!" >&2
 
exit 1

fi

#Анализируем ошибки (HTTP-статусы 4xx и 5xx)

echo "Список ошибок сервера/приложения (4xx и 5xx):" > "$OUTPUT_FILE"

awk '$9 ~ /^[45][0-9]{2}$/ {print $9, $7}' "$LOG_FILE" | sort | uniq -c | sort -nr >> "$OUTPUT_FILE"

echo "Анализ завершен. Результаты сохранены в $OUTPUT_FILE"

Вывод:

badger@ubuntu:~$ cat loganalysis3

Список ошибок сервера/приложения (4xx и 5xx):

     31 404 /
      5 404 /robots.txt
      5 404 /1
      4 400 /wp-admin/admin-ajax.php?page=301bulkoptions
      2 404 /.well-known/security.txt
      2 404 /sitemap.xml
      2 404 /admin/config.php
      1 500 /wp-includes/ID3/comay.php
      1 500 /wp-content/uploads/2018/08/seo_script.php
      1 500 /wp-content/plugins/uploadify/includes/check.php
      1 499 /wp-cron.php?doing_wp_cron=1565803543.6812090873718261718750
      1 499 /wp-cron.php?doing_wp_cron=1565760219.4257180690765380859375
      1 405 /
      1 404 /wp-content/plugins/uploadify/readme.txt
      1 404 /webdav/
      1 404 /manager/html
      1 404 /favicon.ico
      1 403 /wp-content/themes/llorix-one-lite/fonts/fontawesome-webfont.eot?
      1 400 /w00tw00t.at.ISC.SANS.DFind:)
      1 400 http://110.249.212.46/testget?q=23333&port=80
      1 400 http://110.249.212.46/testget?q=23333&port=443


Задание 4. Вывести список всех кодов HTTP ответа с указанием их кол-ва.

Скрипт:
#!/bin/bash

#Входной файл

input_file="/home/badger/access-4560-644067.log"

#Выходной файл

output_file="/home/badger/loganalysis4"

#Проверяем, существует ли входной файл

if [ ! -f "$input_file" ]; 

then

echo "Ошибка: файл $input_file не найден!" >&2

exit 1

fi

#Извлекаем коды HTTP ответов, подсчитываем их и сохраняем в выходной файл

awk '{print $9}' "$input_file" | grep -E "^[0-9]{3}$" | sort | uniq -c | sort -nr > "$output_file"


echo "Анализ завершен. Результаты сохранены в $output_file"

Вывод:

badger@ubuntu:~$ ./script4.sh

Анализ завершен. Результаты сохранены в /home/badger/loganalysis4

badger@ubuntu:~$ cat loganalysis4

    498 200
     95 301
     51 404
      7 400
      3 500
      2 499
      1 405
      1 403
      1 304
