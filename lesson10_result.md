# Домашнее задание к занятию "Компьютерные сети. Лекция 1"



1. Работа c HTTP через телнет.
- Подключитесь утилитой телнет к сайту stackoverflow.com
`telnet stackoverflow.com 80`
- Отправьте HTTP запрос
```
  root@hp:/home/ali# telnet stackoverflow.com 80
  Trying 151.101.129.69...
  Connected to stackoverflow.com.
  Escape character is '^]'.
  GET /questions HTTP/1.0
  HOST: stackoverflow.com

  HTTP/1.1 403 Forbidden
  Connection: close
  
  Ошибка сервера 403 Forbidden означает ограничение или отсутствие доступа к материалу на странице, которую вы пытаетесь загрузить
```

2. Повторите задание 1 в браузере, используя консоль разработчика F12.
```
  Request URL: http://stackoverflow.com/
  Request Method: GET
  Status Code: 307 Internal Redirect
  Referrer Policy: strict-origin-when-cross-origin
  
  код возврата 307 - перенаправила на https://.....
  страница грузилась 10.7s
  самый долгий запрос https://stats.g.doubleclick.net/j/collect?t=dc&aip=1&_r=3&v=1&_v=j99&tid=UA-108242619-1&cid=804196142.1671708086&jid=1175171222&gjid=1094807032&_gid=1985071247.1680445209&_u=SCCACEAAFAAAACAAI~&z=898934885   - 10.05s
  
```

3. Какой IP адрес у вас в интернете?
```
  77.106.87.208

```

5. Какому провайдеру принадлежит ваш IP адрес? Какой автономной системе AS? Воспользуйтесь утилитой `whois`
```
root@hp:/home/ali# whois 77.106.87.208
inetnum:        77.106.80.0 - 77.106.87.255
netname:        ERTH-BARNAUL-NET-80
descr:          CJSC "ER-Telecom Holding" Barnaul branch
descr:          Barnaul, Russia
country:        RU
admin-c:        ERTH22-RIPE
org:            ORG-CHBB2-RIPE
tech-c:         ERTH22-RIPE
status:         ASSIGNED PA
mnt-by:         RAID-MNT
remarks:        INFRA-AW
created:        2022-02-16T05:21:47Z
last-modified:  2022-02-16T05:24:50Z
source:         RIPE

organisation:   ORG-CHBB2-RIPE
org-name:       JSC "ER-Telecom Holding" Barnaul Branch
org-type:       OTHER
descr:          TM DOM.RU, Barnaul ISP
address:        shosse Kosmonavtov, 111
address:        614099 Perm'
address:        Russian Federation
phone:          +7 342 2462 367
fax-no:         +7 342 2195 104
admin-c:        ERTH22-RIPE
tech-c:         ERTH22-RIPE
abuse-c:        RAID1-RIPE
mnt-ref:        RAID-MNT
mnt-by:         RAID-MNT
created:        2009-12-24T10:31:51Z
last-modified:  2019-10-18T12:22:59Z
source:         RIPE # Filtered

role:           Network Operation Center CJSC ER-Telecom Holding Barnaul branch
address:        CJSC "ER-Telecom" Holding", Barnaul branch
address:        shosse Kosmonavtov, 111
address:        614099 Perm'
address:        Russian Federation
phone:          +7 342 2 195 100
fax-no:         +7 342 2 195 100
admin-c:        RAID1-RIPE
tech-c:         RAID1-RIPE
nic-hdl:        ERTH22-RIPE
created:        2009-12-24T10:07:12Z
last-modified:  2019-10-22T09:34:16Z
source:         RIPE # Filtered
mnt-by:         RAID-MNT

% Information related to '77.106.80.0/21AS50512'

route:          77.106.80.0/21
origin:         AS50512
org:            ORG-CHBB2-RIPE
descr:          CJSC "ER-Telecom Holding" Barnaul branch
descr:          Barnaul, Russia
mnt-by:         RAID-MNT
created:        2022-02-16T05:22:13Z
last-modified:  2022-02-16T05:22:13Z
source:         RIPE

organisation:   ORG-CHBB2-RIPE
org-name:       JSC "ER-Telecom Holding" Barnaul Branch
org-type:       OTHER
descr:          TM DOM.RU, Barnaul ISP
address:        shosse Kosmonavtov, 111
address:        614099 Perm'
address:        Russian Federation
phone:          +7 342 2462 367
fax-no:         +7 342 2195 104
admin-c:        ERTH22-RIPE
tech-c:         ERTH22-RIPE
abuse-c:        RAID1-RIPE
mnt-ref:        RAID-MNT
mnt-by:         RAID-MNT
created:        2009-12-24T10:31:51Z
last-modified:  2019-10-18T12:22:59Z
source:         RIPE # Filtered



DOM.RU,  AS50512 

```

7. Через какие сети проходит пакет, отправленный с вашего компьютера на адрес 8.8.8.8? Через какие AS? Воспользуйтесь утилитой `traceroute
```
root@hp:/home/ali# traceroute -A 8.8.8.8
traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
 1  router.asus.com (192.168.0.1) [*]  1.323 ms  1.850 ms  1.815 ms
 2  * * *
 3  lag-3-438.bgw01.barnaul.ertelecom.ru (109.195.40.30) [AS50512]  2.666 ms  2.913 ms  2.861 ms
 4  72.14.215.165 (72.14.215.165) [AS15169]  61.623 ms  66.101 ms  52.618 ms
 5  72.14.215.166 (72.14.215.166) [AS15169]  52.392 ms  52.550 ms  52.533 ms
 6  * * *
 7  72.14.233.94 (72.14.233.94) [AS15169]  51.136 ms 108.170.250.33 (108.170.250.33) [AS15169]  51.120 ms  51.075 ms
 8  108.170.250.99 (108.170.250.99) [AS15169]  49.955 ms 108.170.250.66 (108.170.250.66) [AS15169]  49.647 ms 108.170.250.146 (108.170.250.146) [AS15169]  49.486 ms
 9  142.251.78.106 (142.251.78.106) [AS15169]  57.732 ms 142.251.238.82 (142.251.238.82) [AS15169]  59.739 ms 72.14.234.54 (72.14.234.54) [AS15169]  58.698 ms
10  142.251.238.68 (142.251.238.68) [AS15169]  74.588 ms 142.251.238.66 (142.251.238.66) [AS15169]  74.571 ms 142.251.237.144 (142.251.237.144) [AS15169]  59.841 ms
11  172.253.51.223 (172.253.51.223) [AS15169]  74.510 ms 72.14.235.193 (72.14.235.193) [AS15169]  66.224 ms 142.250.56.217 (142.250.56.217) [AS15169]  67.399 ms
* * *
21  dns.google (8.8.8.8) [AS15169/AS263411]  73.249 ms  67.064 ms  67.097 ms


AS50512 -> AS15169 -> AS263411

```

8. Повторите задание 5(наверное 7) в утилите `mtr`. На каком участке наибольшая задержка - delay?
```
root@hp:/home/ali# mtr -t -y0 8.8.8.8

                                     My traceroute  [v0.94]
hp (192.168.0.8) -> 8.8.8.8                                            2023-04-02T22:27:53+0700
Keys:  Help   Display mode   Restart statistics   Order of fields   quit
                                                       Packets               Pings
 Host                                                Loss%   Snt   Last   Avg  Best  Wrst StDev
 1. AS???    router.asus.com                          0.0%    16    1.6   2.0   1.3   4.2   0.9
 2. AS50512  188.233.215.253                          0.0%    16    2.0   3.0   1.6   8.3   1.9
 3. AS50512  lag-3-438.bgw01.barnaul.ertelecom.ru     0.0%    16    4.6   5.0   1.8  22.6   5.8
 4. AS15169  72.14.215.165                            0.0%    16   53.3  51.2  49.0  56.1   2.5
 5. AS15169  72.14.215.166                            0.0%    16   49.5  54.5  49.3  93.5  12.6
 6. AS15169  108.170.250.129                          0.0%    16   50.3  54.0  50.0  91.1  10.2
 7. AS15169  108.170.250.130                          0.0%    16   49.9  50.3  49.6  51.9   0.7
 8. AS15169  142.250.238.214                          0.0%    16   60.0  60.5  59.7  63.1   1.0
 9. AS15169  142.250.235.62                           0.0%    16   58.8  61.6  58.8  81.3   5.8
10. AS15169  216.239.63.129                           0.0%    16   59.8  60.8  59.4  66.2   1.7
11. (waiting for reply)
12. (waiting for reply)
13. (waiting for reply)
14. (waiting for reply)
15. (waiting for reply)
16. (waiting for reply)
17. (waiting for reply)
18. (waiting for reply)
19. (waiting for reply)
20. (waiting for reply)
21. (waiting for reply)
22. AS15169  dns.google                               0.0%    15   66.4  67.1  66.2  70.2   1.1



Наибольшие задержки на 9ом хопе.
```

9. Какие DNS сервера отвечают за доменное имя dns.google? Какие A записи? Воспользуйтесь утилитой `dig`
```
root@hp:/home/ali# dig NS dns.google

; <<>> DiG 9.16.37-Debian <<>> NS dns.google
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 16469
;; flags: qr rd ra; QUERY: 1, ANSWER: 4, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1280
;; QUESTION SECTION:
;dns.google.			IN	NS

;; ANSWER SECTION:
dns.google.		21600	IN	NS	ns3.zdns.google.
dns.google.		21600	IN	NS	ns1.zdns.google.
dns.google.		21600	IN	NS	ns2.zdns.google.
dns.google.		21600	IN	NS	ns4.zdns.google.



root@hp:/home/ali# dig A dns.google
; <<>> DiG 9.16.37-Debian <<>> A dns.google
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 56972
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1280
;; QUESTION SECTION:
;dns.google.			IN	A

;; ANSWER SECTION:
dns.google.		506	IN	A	8.8.8.8
dns.google.		506	IN	A	8.8.4.4


Ответ:
dns.google.		21600	IN	NS	ns3.zdns.google.
dns.google.		21600	IN	NS	ns1.zdns.google.
dns.google.		21600	IN	NS	ns2.zdns.google.
dns.google.		21600	IN	NS	ns4.zdns.google.

dns.google.		506	IN	A	8.8.8.8
dns.google.		506	IN	A	8.8.4.4

```

11. Проверьте PTR записи для IP адресов из задания 7. Какое доменное имя привязано к IP? Воспользуйтесь утилитой `dig`
```
root@hp:/home/ali# dig -x 8.8.8.8

; <<>> DiG 9.16.37-Debian <<>> -x 8.8.8.8
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 45840
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1280
;; QUESTION SECTION:
;8.8.8.8.in-addr.arpa.		IN	PTR

;; ANSWER SECTION:
8.8.8.8.in-addr.arpa.	68345	IN	PTR	dns.google.

;; Query time: 4 msec
;; SERVER: 192.168.0.1#53(192.168.0.1)
;; WHEN: Sun Apr 02 22:15:03 +07 2023
;; MSG SIZE  rcvd: 73



8.8.8.8.in-addr.arpa
```

