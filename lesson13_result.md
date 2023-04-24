1. Установите плагин Bitwarden для браузера. Зарегестрируйтесь и сохраните несколько паролей.

```
Установил, сохранил.
```

2. Установите Google Authenticator на мобильный телефон. Настройте вход в Bitwarden-акаунт через Google Authenticator OTP.

```
Настроил
```

3. Установите apache2, сгенерируйте самоподписанный сертификат, настройте тестовый сайт для работы по HTTPS.

```
apt install apache2
openssl req -x509 -nodes -days 999000 -newkey rsa:2048 -keyout ./key.key -out ./crt.crt
в дефаултном конфиге apache включим ssl:
SSLEngine on
SSLCertificateFile /etc/ssl/certs/crt.crt
SSLCertificateKeyFile /etc/ssl/private/key.key

/usr/sbin/a2enmod ssl
systemctl restart apache2
```

4. Проверьте на TLS-уязвимости произвольный сайт в интернете (кроме сайтов МВД, ФСБ, МинОбр, НацБанк, РосКосмос, РосАтом, РосНАНО и любых госкомпаний, объектов КИИ, ВПК и т. п.).

```
root@hp:/home/ali# testssl -U --sneaky https://cortel.cloud

###########################################################
    testssl       3.0.4 from https://testssl.sh/

      This program is free software. Distribution and
             modification under GPLv2 permitted.
      USAGE w/o ANY WARRANTY. USE IT AT YOUR OWN RISK!

       Please file bugs @ https://testssl.sh/bugs/

###########################################################

 Using "OpenSSL 1.1.1n  15 Mar 2022" [~81 ciphers]
 on hp:/usr/bin/openssl
 (built: "Feb  5 21:23:17 2023", platform: "debian-amd64")


 Start 2023-04-24 21:25:04        -->> 94.127.93.66:443 (cortel.cloud) <<--

 rDNS (94.127.93.66):    94-127-93-66.accessin.cloud.
 Service detected:       HTTP


 Testing vulnerabilities 

 Heartbleed (CVE-2014-0160)                not vulnerable (OK), no heartbeat extension
 CCS (CVE-2014-0224)                       not vulnerable (OK)
 Ticketbleed (CVE-2016-9244), experiment.  not vulnerable (OK)
 ROBOT                                     not vulnerable (OK)
 Secure Renegotiation (RFC 5746)           supported (OK)
 Secure Client-Initiated Renegotiation     not vulnerable (OK)
 CRIME, TLS (CVE-2012-4929)                not vulnerable (OK)
 BREACH (CVE-2013-3587)                    potentially NOT ok, "gzip" HTTP compression detected. - only supplied "/" tested
                                           Can be ignored for static pages or if no secrets in the page
 POODLE, SSL (CVE-2014-3566)               not vulnerable (OK)
 TLS_FALLBACK_SCSV (RFC 7507)              Downgrade attack prevention supported (OK)
 SWEET32 (CVE-2016-2183, CVE-2016-6329)    not vulnerable (OK)
 FREAK (CVE-2015-0204)                     not vulnerable (OK)
 DROWN (CVE-2016-0800, CVE-2016-0703)      not vulnerable on this host and port (OK)
                                           make sure you don't use this certificate elsewhere with SSLv2 enabled services
                                           https://censys.io/ipv4?q=B14FC64C5E9353D11602CEB8A3EAC739D029521A72AC6CD3DF6F65FDCD2EB733 could help you to find out
 LOGJAM (CVE-2015-4000), experimental      not vulnerable (OK): no DH EXPORT ciphers, no DH key detected with <= TLS 1.2
 BEAST (CVE-2011-3389)                     TLS1: ECDHE-RSA-AES256-SHA AES256-SHA CAMELLIA256-SHA ECDHE-RSA-AES128-SHA AES128-SHA
                                                 CAMELLIA128-SHA 
                                           VULNERABLE -- but also supports higher protocols  TLSv1.1 TLSv1.2 (likely mitigated)
 LUCKY13 (CVE-2013-0169), experimental     potentially VULNERABLE, uses cipher block chaining (CBC) ciphers with TLS. Check patches
 RC4 (CVE-2013-2566, CVE-2015-2808)        no RC4 ciphers detected (OK)


```

5. Установите на Ubuntu SSH-сервер, сгенерируйте новый приватный ключ. Скопируйте свой публичный ключ на другой сервер. Подключитесь к серверу по SSH-ключу.

```
ssh-keygen
ssh-copy-id user@192.168.122.209
ssh user@192.168.122.209
```

6. Переименуйте файлы ключей из задания 5. Настройте файл конфигурации SSH-клиента так, чтобы вход на удалённый сервер осуществлялся по имени сервера.

```
Чтобы клиент подключался по имени достаточно сделать так чтобы его имя разрешалось в ип(в dns, hosts)
```

7. Соберите дамп трафика утилитой tcpdump в формате pcap, 100 пакетов. Откройте файл pcap в Wireshark.

```
tcpdump -i wlp1s0 -c 100 -w result.pcap
```
