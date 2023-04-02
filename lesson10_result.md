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

7. Через какие сети проходит пакет, отправленный с вашего компьютера на адрес 8.8.8.8? Через какие AS? Воспользуйтесь утилитой `traceroute`
8. Повторите задание 5 в утилите `mtr`. На каком участке наибольшая задержка - delay?
9. Какие DNS сервера отвечают за доменное имя dns.google? Какие A записи? Воспользуйтесь утилитой `dig`
10. Проверьте PTR записи для IP адресов из задания 7. Какое доменное имя привязано к IP? Воспользуйтесь утилитой `dig`
