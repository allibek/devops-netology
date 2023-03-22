# Домашнее задание к занятию "Операционные системы. Лекция 2"


1. На лекции мы познакомились с [node_exporter](https://github.com/prometheus/node_exporter/releases). В демонстрации его исполняемый файл запускался в background. Этого достаточно для демо, но не для настоящей production-системы, где процессы должны находиться под внешним управлением. Используя знания из лекции по systemd, создайте самостоятельно простой [unit-файл](https://www.freedesktop.org/software/systemd/man/systemd.service.html) для node_exporter:

    * поместите его в автозагрузку,
    * предусмотрите возможность добавления опций к запускаемому процессу через внешний файл (посмотрите, например, на `systemctl cat cron`),
    * удостоверьтесь, что с помощью systemctl процесс корректно стартует, завершается, а после перезагрузки автоматически поднимается.

         ```
         [Unit]
         Description=node_exporter
         
         [Service]
         User=root
         Group=root
         ExecStart=/home/ali/work/node_exporter-1.5.0.linux-amd64/node_exporter $EXTRA_OPTS
         
         [Install]
         WantedBy=multi-user.target
         
         
         root@hp:/etc/systemd/system# systemctl status ne
         ● ne.service - node-exporter
            Loaded: loaded (/etc/systemd/system/ne.service; disabled; vendor preset: enabled)
            Active: inactive (dead)
         root@hp:/etc/systemd/system# systemctl enable ne
         Created symlink /etc/systemd/system/multi-user.target.wants/ne.service → /etc/systemd/system/ne.service.
         root@hp:/etc/systemd/system# systemctl status ne
         ● ne.service - node-exporter
            Loaded: loaded (/etc/systemd/system/ne.service; enabled; vendor preset: enabled)
            Active: inactive (dead)
         root@hp:/etc/systemd/system# systemctl start ne
         root@hp:/etc/systemd/system# systemctl status ne
         ● ne.service - node_exporter
            Loaded: loaded (/etc/systemd/system/ne.service; enabled; vendor preset: enabled)
            Active: active (running) since Mon 2023-03-20 22:47:07 +07; 4s ago
            Main PID: 8692 (node_exporter)
               Tasks: 5 (limit: 38037)
            Memory: 3.5M
               CPU: 6ms
            CGroup: /system.slice/ne.service
                      └─8692 /home/ali/work/node_exporter-1.5.0.linux-amd64/node_exporter
      мар 20 22:47:07 hp node_exporter[8692]: ts=2023-03-20T15:47:07.925Z caller=node_exporter.go:117 level=info collector=thermal_zone
      мар 20 22:47:07 hp node_exporter[8692]: ts=2023-03-20T15:47:07.925Z caller=node_exporter.go:117 level=info collector=time
      мар 20 22:47:07 hp node_exporter[8692]: ts=2023-03-20T15:47:07.925Z caller=node_exporter.go:117 level=info collector=timex
      мар 20 22:47:07 hp node_exporter[8692]: ts=2023-03-20T15:47:07.925Z caller=node_exporter.go:117 level=info collector=udp_queues
      мар 20 22:47:07 hp node_exporter[8692]: ts=2023-03-20T15:47:07.925Z caller=node_exporter.go:117 level=info collector=uname
      мар 20 22:47:07 hp node_exporter[8692]: ts=2023-03-20T15:47:07.925Z caller=node_exporter.go:117 level=info collector=vmstat
      мар 20 22:47:07 hp node_exporter[8692]: ts=2023-03-20T15:47:07.925Z caller=node_exporter.go:117 level=info collector=xfs
      мар 20 22:47:07 hp node_exporter[8692]: ts=2023-03-20T15:47:07.925Z caller=node_exporter.go:117 level=info collector=zfs
      мар 20 22:47:07 hp node_exporter[8692]: ts=2023-03-20T15:47:07.926Z caller=tls_config.go:232 level=info msg="Listening on" address=[:>
      мар 20 22:47:07 hp node_exporter[8692]: ts=2023-03-20T15:47:07.926Z caller=tls_config.go:235 level=info msg="TLS is disabled." http2
      ```


2. Ознакомьтесь с опциями node_exporter и выводом `/metrics` по-умолчанию. Приведите несколько опций, которые вы бы выбрали для базового мониторинга хоста по CPU, памяти, диску и сети.
     ```
     node_cpu_seconds_total{cpu="....",mode="system"}
     node_memory_MemAvailable_bytes
     node_memory_MemFree_bytes
     node_disk_io_time_seconds_total{device="....."}
     node_network_receive_bytes_total
     node_network_transmit_bytes_total
     ```

3. Установите в свою виртуальную машину [Netdata](https://github.com/netdata/netdata). Воспользуйтесь [готовыми пакетами](https://packagecloud.io/netdata/netdata/install) для установки (`sudo apt install -y netdata`). 
   
   После успешной установки:
    * в конфигурационном файле `/etc/netdata/netdata.conf` в секции [web] замените значение с localhost на `bind to = 0.0.0.0`,
    * добавьте в Vagrantfile проброс порта Netdata на свой локальный компьютер и сделайте `vagrant reload`:

    ```bash
    config.vm.network "forwarded_port", guest: 19999, host: 19999
    ```

    После успешной перезагрузки в браузере *на своем ПК* (не в виртуальной машине) вы должны суметь зайти на `localhost:19999`. Ознакомьтесь с метриками, которые по умолчанию собираются Netdata и с комментариями, которые даны к этим метрикам.

4. Можно ли по выводу `dmesg` понять, осознает ли ОС, что загружена не на настоящем оборудовании, а на системе виртуализации?

   ```
   root@controller:/home/user# dmesg | grep virt
   [    0.006319] Booting paravirtualized kernel on KVM
   [    1.455541] systemd[1]: Detected virtualization kvm.
   root@controller:/home/user# 

   ```

5. Как настроен sysctl `fs.nr_open` на системе по-умолчанию? Определите, что означает этот параметр. Какой другой существующий лимит не позволит достичь такого числа (`ulimit --help`)?
   ```
   Параметр означает максимальное число открытых файловых дескрипторов
   
   root@controller:/home/user# cat /proc/sys/fs/nr_open 
   1048576
   root@controller:/home/user# ulimit -n
   1024
   root@controller:/home/user# 
   
   ```

6. Запустите любой долгоживущий процесс (не `ls`, который отработает мгновенно, а, например, `sleep 1h`) в отдельном неймспейсе процессов; покажите, что ваш процесс работает под PID 1 через `nsenter`. Для простоты работайте в данном задании под root (`sudo -i`). Под обычным пользователем требуются дополнительные опции (`--map-root-user`) и т.д.

7. Найдите информацию о том, что такое `:(){ :|:& };:`. Запустите эту команду в своей виртуальной машине Vagrant с Ubuntu 20.04 (**это важно, поведение в других ОС не проверялось**). Некоторое время все будет "плохо", после чего (минуты) – ОС должна стабилизироваться. Вызов `dmesg` расскажет, какой механизм помог автоматической стабилизации.  
Как настроен этот механизм по-умолчанию, и как изменить число процессов, которое можно создать в сессии?

*В качестве решения ответьте на вопросы и опишите каким образом эти ответы были получены*

----


