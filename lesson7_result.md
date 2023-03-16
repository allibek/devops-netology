# Домашнее задание к занятию "Операционные системы. Лекция 1"

1. Какой системный вызов делает команда `cd`?             
        chdir("/tmp")                           = 0              

        root@hp:/home/ali# strace bash -c 'cd /tmp' 2>&1 |grep tmp 
        execve("/usr/bin/bash", ["bash", "-c", "cd /tmp"], 0x7fff84188390 /* 42 vars */) = 0
        stat("/tmp", {st_mode=S_IFDIR|S_ISVTX|0777, st_size=4096, ...}) = 0
        chdir("/tmp")                           = 0
        root@hp:/home/ali# 


2. Используя `strace` выясните, где находится база данных `file`, на основании которой она делает свои догадки.

        root@hp:/home/ali# strace file /dev/tty 2>&1 | grep ^stat
        stat("/root/.magic.mgc", 0x7fff76e45d90) = -1 ENOENT (Нет такого файла или каталога)
        stat("/root/.magic", 0x7fff76e45d90)    = -1 ENOENT (Нет такого файла или каталога)
        stat("/etc/magic", {st_mode=S_IFREG|0644, st_size=111, ...}) = 0
        
        root@hp:/home/ali# strace file /dev/sda 2>&1 | grep ^stat
        stat("/root/.magic.mgc", 0x7ffc7f3aaab0) = -1 ENOENT (Нет такого файла или каталога)
        stat("/root/.magic", 0x7ffc7f3aaab0)    = -1 ENOENT (Нет такого файла или каталога)
        stat("/etc/magic", {st_mode=S_IFREG|0644, st_size=111, ...}) = 0

        root@hp:/home/ali# strace file /bin/bash 2>&1 | grep ^stat
        stat("/root/.magic.mgc", 0x7ffd87360f80) = -1 ENOENT (Нет такого файла или каталога)
        stat("/root/.magic", 0x7ffd87360f80)    = -1 ENOENT (Нет такого файла или каталога)
        stat("/etc/magic", {st_mode=S_IFREG|0644, st_size=111, ...}) = 0
        root@hp:/home/ali# 


3. Предположим, приложение пишет лог в текстовый файл. Этот файл оказался удален (deleted в lsof), однако возможности сигналом сказать приложению переоткрыть файлы или просто перезапустить приложение – нет. Так как приложение продолжает писать в удаленный файл, место на диске постепенно заканчивается. Основываясь на знаниях о перенаправлении потоков предложите способ обнуления открытого удаленного файла (чтобы освободить место на файловой системе).

        root@hp:/home/ali# while true; do echo "hello"; sleep 2; done > result.txt &
        [1] 4738
        root@hp:/home/ali# rm result.txt
        root@hp:/home/ali# cat /proc/4738/fd/1
        hello
        hello
        .....
        .....
        hello
        hello
        root@hp:/home/ali# echo "" > /proc/4738/fd/1
        root@hp:/home/ali# cat /proc/4738/fd/1
        
        hello
        root@hp:/home/ali# kill -s 9 4738



4. Занимают ли зомби-процессы какие-то ресурсы в ОС (CPU, RAM, IO)?

        Занимают только PID в системе.


5. В iovisor BCC есть утилита `opensnoop`...

        PID    COMM               FD ERR PATH
        1302   Chrome_IOThread   430   0 /dev/shm/.com.google.Chrome.VVktmF
        1302   Chrome_IOThread   431   0 /dev/shm/.com.google.Chrome.VVktmF
        1302   Chrome_IOThread   430   0 /dev/shm/.com.google.Chrome.VWXAqG
        1302   Chrome_IOThread   431   0 /dev/shm/.com.google.Chrome.VWXAqG
        1302   Chrome_IOThread   430   0 /dev/shm/.com.google.Chrome.DBLd6C
        1302   Chrome_IOThread   431   0 /dev/shm/.com.google.Chrome.DBLd6C
        1302   Chrome_IOThread   430   0 /dev/shm/.com.google.Chrome.QCb0IC
        1302   Chrome_IOThread   431   0 /dev/shm/.com.google.Chrome.QCb0IC
        1302   Chrome_IOThread   430   0 /dev/shm/.com.google.Chrome.hST3BG
        1302   Chrome_IOThread   431   0 /dev/shm/.com.google.Chrome.hST3BG
        1302   Chrome_IOThread   430   0 /dev/shm/.com.google.Chrome.9RB4LC
        1302   Chrome_IOThread   431   0 /dev/shm/.com.google.Chrome.9RB4LC


6. Какой системный вызов использует `uname -a`? Приведите цитату из man по этому системному вызову, где описывается альтернативное местоположение в `/proc`, где можно узнать версию ядра и релиз ОС.

7. Чем отличается последовательность команд через `;` и через `&&` в bash?
 
        ; - команды выполняются поочереди независимо от их результата. 
        && - каждая следующая команда выполняется только если предыдущая завершилась успешно

    Есть ли смысл использовать в bash `&&`, если применить `set -e`?
    
        -e  Exit immediately if a command exits with a non-zero status.
        set -e остановит скрипт целиком при команде с не 0 кодом, смысл && пропадает.

8. Из каких опций состоит режим bash `set -euxo pipefail` и почему его хорошо было бы использовать в сценариях?


        -e  Exit immediately if a command exits with a non-zero status.
        -u  Treat unset variables as an error when substituting.
        -x  Print commands and their arguments as they are executed.
        pipefail     the return value of a pipeline is the status of the last command to exit with a non-zero status, or zero if no command exited with a non-zero status
        Выход во время ошибки, считать за ошибку подстановку несуществующей переменной, вывод команд и аргументов во время выполнения, выводить exit код выполнения кажлой команды. Опциип полезны при отладке скрипта. 


9. Используя `-o stat` для `ps`, определите, какой наиболее часто встречающийся статус у процессов в системе. В `man ps` ознакомьтесь (`/PROCESS STATE CODES`) что значат дополнительные к основной заглавной буквы статуса процессов. Его можно не учитывать при расчете (считать S, Ss или Ssl равнозначными).

        root@hp:/home/ali# ps -o stat
        STAT
        S
        S
        R+
        root@hp:/home/ali# 

