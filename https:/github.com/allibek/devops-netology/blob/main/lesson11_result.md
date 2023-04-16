1. Подключитесь к публичному маршрутизатору в интернет. Найдите маршрут к вашему публичному IP

2. Создайте dummy0 интерфейс в Ubuntu. Добавьте несколько статических маршрутов. Проверьте таблицу маршрутизации.
```
root@hp:/home/ali# modprobe -v dummy numdummies=1
root@hp:/home/ali# ip addr add 10.0.0.1/24 dev dummy0
root@hp:/home/ali# ip link set dev dummy0 up
root@hp:/home/ali# ip route add 10.10.10.0/24 dev dummy0
root@hp:/home/ali# ip route add 20.20.20.0/24 dev dummy0
root@hp:/home/ali# routel | grep dummy
      10.0.0.0/ 24                        10.0.0.1   kernel     link dummy0 
    10.10.10.0/ 24                                              link dummy0 
    20.20.20.0/ 24                                              link dummy0 
       10.0.0.0          broadcast        10.0.0.1   kernel     link dummy0 local
       10.0.0.1              local        10.0.0.1   kernel     host dummy0 local
     10.0.0.255          broadcast        10.0.0.1   kernel     link dummy0 local
```
3. Проверьте открытые TCP порты в Ubuntu, какие протоколы и приложения используют эти порты? Приведите несколько примеров.

4. Проверьте используемые UDP сокеты в Ubuntu, какие протоколы и приложения используют эти порты?

5. Используя diagrams.net, создайте L3 диаграмму вашей домашней сети или любой другой сети, с которой вы работали.
