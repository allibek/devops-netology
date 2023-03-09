1) Установка VM:
<img src="/lesson5_img/1.png">

<img src="/lesson5_img/2.png">

<img src="/lesson5_img/3.png">

<img src="/lesson5_img/4.png">


2) Ресурсы выделеные по-умолчанию: 2/1024
<img src="/lesson5_img/5.png">

3) Добавление оперативной памяти и CPU:


Vagrant.configure("2") do |config|

    config.vm.box = "bento/ubuntu-20.04"

	config.vm.provider "virtualbox" do |v|
	
		v.memory = 2024
	    
	    	v.cpus = 4
    end
end

<img src="/lesson5_img/res.png">


4)Подключение по ssh:
<img src="/lesson5_img/6.png">


5) export $HISTSIZE=....
   Строка 941. искать: /HISTSIZE
   ignoreboth совпадающие строки в истории не сохраняются
<img src="/lesson5_img/7.png">

6) {} - список
<img src="/lesson5_img/8.png">

7)touch {1..100000}.txt

touch {1..300000}.txt
bash: /usr/bin/touch: Слишком длинный список аргументов


8) [[ -d /tmp ]] - проверяет что директория существует

11) <img src="/lesson5_img/11.png">
