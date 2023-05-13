### Домашнее задание к занятию "5.4. Практические навыки работы с Docker"

## Задача 1
В данном задании вы научитесь изменять существующие Dockerfile, адаптируя их под нужный инфраструктурный стек.
Измените базовый образ предложенного Dockerfile на Arch Linux c сохранением его функциональности.

```
root@hp:/home/ali/devops-netology/docker2# cat Dockerfile 
FROM archlinux:latest

RUN yes | pacman -Sy community/ponysay

ENTRYPOINT ["/usr/bin/ponysay"]
CMD ["Hey, netology”]
```
```
root@hp:/home/ali/devops-netology/docker2# docker logs bafd34b11c2d
 ____________________________ 
< /bin/sh ["Hey, netology”]  >
 ---------------------------- 
                           \                         
                            \                        
                             \                       
                              \                      
                               \     ▄▄▄▄▄▄▄▄        
                               ▄▄▄▄▄▄████████▄▄▄     
                              ▄▄▄████████████▄▄▄█▄   
                   ▄▄▄ ▄▄▄▄  ▄████▄▄██▄█▄▄▄▄███▄▄▀   
                   ███▄█▄█▄▄▄███▄███▄▄▄▄▄███▀▀▄█▄▄   
                    ▀▄█▄███████▄███▄▄▄▄████▄▄  ▀▄█   
                   █▄█▄██████████▄▄█▄█▄████▄▄   ▀█   
                   ▀▄█▄▄▄█▄▄█▄▄███▄▄▄█▄▄██▄▄▄        
             ▄▄▄▄▄▄▄▄█▄██▄███████████████████        
            ██████████▄██▄██████▄████▄▄▄▄█▀▀         
          ▄▄███████▄▀▀████████████▄███               
         █▄▄▄███████ ███▄▄▄███▄▄▄██▄██               
         ▀ ██████▄██ █▄▄█▄▄▄█▄█▄▄████                
         ▄▄███████▄▀  █▄▄█▄▄██████▄█                 
      ▄▄▄██████▄▀ ▀ ▄▄▄█▄███▀▀██████                 
   ▄▄▄███▄▄▄▄█▄▀    ███▄███   ██████                 
     ▀▀▀▀▄█▄▄▀     ▄▄██████   ███████                
        ▀▀▀        ████████   ███████▄               
                  ▄▄███████   █████████              
                  ██████▀▀▀   ▀▄███▄█▀▀              
                  ▀▀▀▀▀▀       ▀▀▀▀▀▀                
                                                    
```

```
образ: allibek/repo1:pony_say
```


## Задача 2
В данной задаче вы составите несколько разных Dockerfile для проекта Jenkins, опубликуем образ в dockerhub.io и посмотрим логи этих контейнеров.

Составьте 2 Dockerfile:

Общие моменты:

Образ должен запускать Jenkins server
Спецификация первого образа:

Базовый образ - amazoncorreto
Присвоить образу тэг ver1
Спецификация второго образа:

Базовый образ - ubuntu:latest
Присвоить образу тэг ver2
Соберите 2 образа по полученным Dockerfile
```
FROM ubuntu:22.04

RUN apt update && apt install -y curl openjdk-11-jre && \
	curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null  && \
	echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | tee /etc/apt/sources.list.d/jenkins.list > /dev/null && \
	apt update && \
	apt install -y jenkins


# web ui
EXPOSE 8080

# slave agents:
EXPOSE 50000

ENTRYPOINT ["/usr/bin/java", "-Djava.awt.headless=true", "-jar", "/usr/share/java/jenkins.war", "--webroot=/var/cache/jenkins/war", "--httpPort=8080"]

```
```
образ: allibek/repo1:ubuntu_jenkins
```

## Задача 3
В данном задании вы научитесь:

объединять контейнеры в единую сеть
исполнять команды "изнутри" контейнера
Для выполнения задания вам нужно:

Написать Dockerfile:

Использовать образ https://hub.docker.com/_/node как базовый
Установить необходимые зависимые библиотеки для запуска npm приложения https://github.com/simplicitesoftware/nodejs-demo
Выставить у приложения (и контейнера) порт 3000 для прослушки входящих запросов
Соберите образ и запустите контейнер в фоновом режиме с публикацией порта
Запустить второй контейнер из образа ubuntu:latest

Создайть docker network и добавьте в нее оба запущенных контейнера

Используя docker exec запустить командную строку контейнера ubuntu в интерактивном режиме

Используя утилиту curl вызвать путь / контейнера с npm приложением

Для получения зачета, вам необходимо предоставить:

Наполнение Dockerfile с npm приложением
Скриншот вывода вызова команды списка docker сетей (docker network cli)
Скриншот вызова утилиты curl с успешным ответом

