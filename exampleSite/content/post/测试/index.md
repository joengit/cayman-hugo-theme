---
title: "树莓派"
date: 2020-09-20
tags: [树莓派]
---


1. 前往[**官网下载页面**](https://www.raspberrypi.org/downloads/raspberry-pi-os/ "官网下载页面")下载最新版Raspberry Pi OS

2. 在网上下载**win32diskimager**，刷写系统至SD卡
	> 如果想要用SSH，记得在SD卡根目录创建一个‘SSH’的空文件，这样树莓派会自动开启SSH

3. 打开ssh&vnc以及调整分辨率

	```
	sudo raspi-config
	```

4. 更换软件源为
	`http://mirrors.aliyun.com/raspbian/raspbian`

	```
	sudo nano /etc/apt/sources.list
	```

1. 更换系统源为
	`http://chinanet.mirrors.ustc.edu.cn/archive.raspberrypi.org/debian/`

	```
	sudo nano /etc/apt/sources.list.d/raspi.list
	```

2. 更新软件

	```
	sudo apt-get update
	sudo apt-get upgrade
	```

3. 安装远程桌面

	```
	sudo apt-get install xrdp
	```

4. ~~安装输入法
```sudo apt-get install fcitx fcitx-sunpinyin```~~

1. 部署lnmp/lamp等环境

	**方法1：直接部署**
	
	1. 安装软件

		```
		#nginx/apache2
		sudo apt-get install nginx
		sudo apt-get install apache2

		#mariadb
		sudo apt-get install mariadb-server mariadb-client
		sudo mysql_secure_installation

		#php 7.3
		sudo apt install -y -t buster php7.3-fpm php7.3-curl php7.3-gd php7.3-intl php7.3-mbstring php7.3-mysql php7.3-imap php7.3-opcache php7.3-sqlite3 php7.3-xml php7.3-xmlrpc php7.3-zip
		```

	2. 配置PHP

		- nginx
			```
			sudo nano /etc/nginx/sites-enabled/default
			```
			> 找到# pass PHP scripts to FastCGI server， 在后面加入以下代码:
			> ``` {.line-numbers}
			>	 location ~ \.php$ {   
			>		 include snippets/fastcgi-php.conf;		
			>		 fastcgi_pass unix:/run/php/php7.3-fpm.sock;	
			>	 }
			> ```
			> > Add index.php to the list if you are using PHP

			```
			sudo nginx -s reload
			```

		- apache

			```
			sudo apt-get install libapache2-mod-php
			```

	3. 安装PHPMYADMIN
		`sudo apt-get install phpmyadmin`
		安装完成后，`sudo mysql -u root` ，登入数据库后，依次执行以下SQL：
		```
		use mysql #切换到mysql数据库
		update user set plugin='mysql_native_password'; #修改plugin字段
		flush privileges; #刷新权限
		exit; #退出数据库
		```
		把phpmyadmin链接到/var/www/html目录下
		```
		sudo ln -s /usr/share/phpmyadmin /var/www/html
		```

	**方法2：宝塔**

	- 32位安装：
		`wget -O install.sh http://download.bt.cn/install/install-ubuntu.sh && sudo bash install.sh`
	- 64位安装：
		Ubuntu/Deepin/Debian安装命令：
		```wget -O install.sh http://download.bt.cn/install/install-ubuntu_6.0.sh && sudo bash install.sh```

		试验性Centos/Ubuntu/Debian安装命令（独立运行环境（py3.7） 可能存在少量兼容性问题 不断优化中）：
	```curl -sSO http://download.bt.cn/install/install_panel.sh && bash install_panel.sh```

	**方法3：DOCKER安装**
	（待补充）
