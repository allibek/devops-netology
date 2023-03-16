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


1. Предположим, приложение пишет лог в текстовый файл. Этот файл оказался удален (deleted в lsof), однако возможности сигналом сказать приложению переоткрыть файлы или просто перезапустить приложение – нет. Так как приложение продолжает писать в удаленный файл, место на диске постепенно заканчивается. Основываясь на знаниях о перенаправлении потоков предложите способ обнуления открытого удаленного файла (чтобы освободить место на файловой системе).

1. Занимают ли зомби-процессы какие-то ресурсы в ОС (CPU, RAM, IO)?

1. В iovisor BCC есть утилита `opensnoop`:
    ```bash
    root@vagrant:~# dpkg -L bpfcc-tools | grep sbin/opensnoop
    /usr/sbin/opensnoop-bpfcc
    ```
    На какие файлы вы увидели вызовы группы `open` за первую секунду работы утилиты? Воспользуйтесь пакетом `bpfcc-tools` для Ubuntu 20.04. Дополнительные [сведения по установке](https://github.com/iovisor/bcc/blob/master/INSTALL.md).

1. Какой системный вызов использует `uname -a`? Приведите цитату из man по этому системному вызову, где описывается альтернативное местоположение в `/proc`, где можно узнать версию ядра и релиз ОС.

1. Чем отличается последовательность команд через `;` и через `&&` в bash? Например:
    ```bash
    root@netology1:~# test -d /tmp/some_dir; echo Hi
    Hi
    root@netology1:~# test -d /tmp/some_dir && echo Hi
    root@netology1:~#
    ```
    Есть ли смысл использовать в bash `&&`, если применить `set -e`?

1. Из каких опций состоит режим bash `set -euxo pipefail` и почему его хорошо было бы использовать в сценариях?

1. Используя `-o stat` для `ps`, определите, какой наиболее часто встречающийся статус у процессов в системе. В `man ps` ознакомьтесь (`/PROCESS STATE CODES`) что значат дополнительные к основной заглавной буквы статуса процессов. Его можно не учитывать при расчете (считать S, Ss или Ssl равнозначными).

----
