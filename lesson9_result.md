# Домашнее задание к занятию "Файловые системы"


1. Узнайте о [sparse](https://ru.wikipedia.org/wiki/%D0%A0%D0%B0%D0%B7%D1%80%D0%B5%D0%B6%D1%91%D0%BD%D0%BD%D1%8B%D0%B9_%D1%84%D0%B0%D0%B9%D0%BB) (разряженных) файлах.

    ```
    изучил...
    ```

2. Могут ли файлы, являющиеся жесткой ссылкой на один объект, иметь разные права доступа и владельца? Почему?

    ```
    Не могут, жесткая сылка является темже файлом, поскольку они имеют одинаковый inode.
    ```

3. Сделайте `vagrant destroy` на имеющийся инстанс Ubuntu. Создайте новую виртуальную машину с двумя дополнительными неразмеченными дисками по 2.5 Гб.

    ```
    root@controller:/home/user# lsblk
    NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    sda      8:0    0   40G  0 disk 
    ├─sda1   8:1    0   39G  0 part /
    ├─sda2   8:2    0    1K  0 part 
    └─sda5   8:5    0  975M  0 part [SWAP]
    sdb      8:16   0  2,5G  0 disk 
    sdc      8:32   0  2,5G  0 disk 
    sr0     11:0    1 1024M  0 rom  
    ```

4. Используя `fdisk`, разбейте первый диск на 2 раздела: 2 Гб, оставшееся пространство.

    ```
    root@controller:/home/user# lsblk
    NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    sda      8:0    0   40G  0 disk 
    ├─sda1   8:1    0   39G  0 part /
    ├─sda2   8:2    0    1K  0 part 
    └─sda5   8:5    0  975M  0 part [SWAP]
    sdb      8:16   0  2,5G  0 disk 
    └─sdb1   8:17   0    2G  0 part 
    └─sdb2   8:18   0  511M  0 part 
    sdc      8:32   0  2,5G  0 disk 
    sr0     11:0    1 1024M  0 rom 
    ```

5. Используя `sfdisk`, перенесите данную таблицу разделов на второй диск.

    ```
    /usr/sbin/sfdisk -d /dev/sdb | /usr/sbin/sfdisk /dev/sdc
    root@controller:/home/user# lsblk
    NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    sda      8:0    0   40G  0 disk 
    ├─sda1   8:1    0   39G  0 part /
    ├─sda2   8:2    0    1K  0 part 
    └─sda5   8:5    0  975M  0 part [SWAP]
    sdb      8:16   0  2,5G  0 disk 
    ├─sdb1   8:17   0    2G  0 part 
    └─sdb2   8:18   0  511M  0 part 
    sdc      8:32   0  2,5G  0 disk 
    ├─sdc1   8:33   0    2G  0 part 
    └─sdc2   8:34   0  511M  0 part 
    sr0     11:0    1 1024M  0 rom 
    ```

6. Соберите `mdadm` RAID1 на паре разделов 2 Гб.

    ```
    /usr/sbin/mdadm --create /dev/md0 -l 1 -n 2 /dev/sdb1 /dev/sdc1
    root@controller:/home/user# lsblk
    NAME    MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
    sda       8:0    0   40G  0 disk  
    ├─sda1    8:1    0   39G  0 part  /
    ├─sda2    8:2    0    1K  0 part  
    └─sda5    8:5    0  975M  0 part  [SWAP]
    sdb       8:16   0  2,5G  0 disk  
    ├─sdb1    8:17   0    2G  0 part  
    │ └─md0   9:0    0    2G  0 raid1 
    └─sdb2    8:18   0  511M  0 part  
    sdc       8:32   0  2,5G  0 disk  
    ├─sdc1    8:33   0    2G  0 part  
    │ └─md0   9:0    0    2G  0 raid1 
    └─sdc2    8:34   0  511M  0 part  
    sr0      11:0    1 1024M  0 rom   
    ```

7. Соберите `mdadm` RAID0 на второй паре маленьких разделов.

    ```
    /usr/sbin/mdadm --create /dev/md1 -l 0 -n 2 /dev/sdb2 /dev/sdc2
    root@controller:/home/user# lsblk
    NAME    MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
    sda       8:0    0   40G  0 disk  
    ├─sda1    8:1    0   39G  0 part  /
    ├─sda2    8:2    0    1K  0 part  
    └─sda5    8:5    0  975M  0 part  [SWAP]
    sdb       8:16   0  2,5G  0 disk  
    ├─sdb1    8:17   0    2G  0 part  
    │ └─md0   9:0    0    2G  0 raid1 
    └─sdb2    8:18   0  511M  0 part  
      └─md1   9:1    0 1018M  0 raid0 
    sdc       8:32   0  2,5G  0 disk  
    ├─sdc1    8:33   0    2G  0 part  
    │ └─md0   9:0    0    2G  0 raid1 
    └─sdc2    8:34   0  511M  0 part  
      └─md1   9:1    0 1018M  0 raid0 
    sr0      11:0    1 1024M  0 rom  
    
    ```

8. Создайте 2 независимых PV на получившихся md-устройствах.

    ```
    root@controller:/home/user# /usr/sbin/pvcreate /dev/md0
      Physical volume "/dev/md0" successfully created.
    root@controller:/home/user# /usr/sbin/pvcreate /dev/md1
      Physical volume "/dev/md1" successfully created.
    root@controller:/home/user# 

    ```

9. Создайте общую volume-group на этих двух PV.

    ```
    root@controller:/home/user# /usr/sbin/vgcreate vg_homework /dev/md0 /dev/md1
      Volume group "vg_homework" successfully created
    root@controller:/home/user# 


    ```
    
10. Создайте LV размером 100 Мб, указав его расположение на PV с RAID0.

    ```
    root@controller:/home/user# /usr/sbin/lvcreate -n lv100 -L100M vg_homework /dev/md1 
    Logical volume "lv100" created.
    root@controller:/home/user# lsblk
    NAME                    MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
    sda                       8:0    0   40G  0 disk  
    ├─sda1                    8:1    0   39G  0 part  /
    ├─sda2                    8:2    0    1K  0 part  
    └─sda5                    8:5    0  975M  0 part  [SWAP]
    sdb                       8:16   0  2,5G  0 disk  
    ├─sdb1                    8:17   0    2G  0 part  
    │ └─md0                   9:0    0    2G  0 raid1 
    └─sdb2                    8:18   0  511M  0 part  
    └─md1                   9:1    0 1018M  0 raid0 
        └─vg_homework-lv100 253:0    0  100M  0 lvm   
    sdc                       8:32   0  2,5G  0 disk  
    ├─sdc1                    8:33   0    2G  0 part  
    │ └─md0                   9:0    0    2G  0 raid1 
    └─sdc2                    8:34   0  511M  0 part  
    └─md1                   9:1    0 1018M  0 raid0 
        └─vg_homework-lv100 253:0    0  100M  0 lvm   
    sr0                      11:0    1 1024M  0 rom  
    ```

11. Создайте `mkfs.ext4` ФС на получившемся LV.

    ```
    /usr/sbin/mkfs.ext4 /dev/vg_homework/lv100
    ```

12. Смонтируйте этот раздел в любую директорию, например, `/tmp/new`.

    ```
    root@controller:/home/user# mkdir /tmp/new
    root@controller:/home/user# mount /dev/vg_homework/lv100 /tmp/new/
    root@controller:/home/user# mount |grep /tmp/new
    /dev/mapper/vg_homework-lv100 on /tmp/new type ext4 (rw,relatime,stripe=1024)

    ```

13. Поместите туда тестовый файл, например `wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz`.

    ```
    
    ```

14. Прикрепите вывод `lsblk`.

    ```
    
    ```
    
15. Протестируйте целостность файла:

    ```bash
    root@vagrant:~# gzip -t /tmp/new/test.gz
    root@vagrant:~# echo $?
    0
    ```

16. Используя pvmove, переместите содержимое PV с RAID0 на RAID1.

    ```
    
    ```

17. Сделайте `--fail` на устройство в вашем RAID1 md.

    ```
    
    ```

18. Подтвердите выводом `dmesg`, что RAID1 работает в деградированном состоянии.

    ```
    
    ```

19. Протестируйте целостность файла, несмотря на "сбойный" диск он должен продолжать быть доступен:

    ```bash
    root@vagrant:~# gzip -t /tmp/new/test.gz
    root@vagrant:~# echo $?
    0
    ```
