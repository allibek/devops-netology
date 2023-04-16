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
```
root@hp:/home/ali# ss -ltpn
State     Recv-Q     Send-Q         Local Address:Port          Peer Address:Port    Process                                                      
LISTEN    0          4096                 0.0.0.0:111                0.0.0.0:*        users:(("rpcbind",pid=530,fd=4),("systemd",pid=1,fd=35))    
LISTEN    0          128                127.0.0.1:631                0.0.0.0:*        users:(("cupsd",pid=793,fd=7))                              
LISTEN    0          10                   0.0.0.0:7070               0.0.0.0:*        users:(("anydesk",pid=725,fd=32))                           
LISTEN    0          4096               127.0.0.1:45567              0.0.0.0:*        users:(("containerd",pid=719,fd=13))                        
LISTEN    0          4096                       *:9100                     *:*        users:(("node_exporter",pid=597,fd=3))                      
LISTEN    0          4096                    [::]:111                   [::]:*        users:(("rpcbind",pid=530,fd=6),("systemd",pid=1,fd=37))    
LISTEN    0          128                    [::1]:631                   [::]:*        users:(("cupsd",pid=793,fd=6))


631  - служба печати
9100 - node exporter
7070 - anydesk
45567 - docker
```

4. Проверьте используемые UDP сокеты в Ubuntu, какие протоколы и приложения используют эти порты?
```
root@hp:/home/ali# ss -lupn
State     Recv-Q     Send-Q         Local Address:Port          Peer Address:Port    Process                                                      
UNCONN    0          0                    0.0.0.0:111                0.0.0.0:*        users:(("rpcbind",pid=530,fd=5),("systemd",pid=1,fd=36))    
UNCONN    0          0                    0.0.0.0:631                0.0.0.0:*        users:(("cups-browsed",pid=868,fd=7))                       
UNCONN    0          0                    0.0.0.0:50001              0.0.0.0:*        users:(("anydesk",pid=725,fd=46))                           
UNCONN    0          0                224.0.0.251:5353               0.0.0.0:*        users:(("chrome",pid=2163,fd=68))                           
UNCONN    0          0                224.0.0.251:5353               0.0.0.0:*        users:(("chrome",pid=2163,fd=52))                           
UNCONN    0          0                224.0.0.251:5353               0.0.0.0:*        users:(("chrome",pid=2118,fd=486))                          
UNCONN    0          0                224.0.0.251:5353               0.0.0.0:*        users:(("chrome",pid=2118,fd=184))                          
UNCONN    0          0                    0.0.0.0:5353               0.0.0.0:*        users:(("avahi-daemon",pid=592,fd=12))                      
UNCONN    0          0                    0.0.0.0:44018              0.0.0.0:*        users:(("avahi-daemon",pid=592,fd=14))                      
UNCONN    0          0                       [::]:111                   [::]:*        users:(("rpcbind",pid=530,fd=7),("systemd",pid=1,fd=38))    
UNCONN    0          0                       [::]:5353                  [::]:*        users:(("avahi-daemon",pid=592,fd=13))                      
UNCONN    0          0                       [::]:58745                 [::]:*        users:(("avahi-daemon",pid=592,fd=15))   

631   - служба печати
5353  - chrome
50001 - anydesk

```

5. Используя diagrams.net, создайте L3 диаграмму вашей домашней сети или любой другой сети, с которой вы работали.
