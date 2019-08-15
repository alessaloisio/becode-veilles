# Create a Wordpress environment on DOCKER

## What's & Why DOCKER ?

Create containers with an operating system, wich version of tools you need, you can do some benchmark easily to determine the best tools for your softwares.

## What we need ?

- OS : **debian**, centOs, Ubuntu Server ..
- HTTP Server : **apache2**, nginx ..

- For Wordpress :
  - PHP
  - MariaDb
  - [Wordpress](https://fr.wordpress.org/download/)

## Let's Go

1. Install DOCKER

```shell
sudo apt install docker.io docker-compose
sudo usermod -aG docker $USER
```

2. Get the [debian](https://hub.docker.com/_/debian?tab=description) image

```shell
$ docker pull debian:latest
```

3. Create and access Debian container

```shell
$ docker run -t -i debian:latest
```

To get your container ip address

```
$ ip address show
```

4. Install APACHE2 + PHP + MariaDB

```
$ apt update && apt -y upgrade

$ apt -y install wget vim apache2 php php-common libapache2-mod-php mariadb-server php-mysql

$ echo "<?php phpinfo(); ?>" | tee /var/www/html/phpinfo.php
$ (/etc/init.d/apache2 restart)

$ a2enmod php7.3
$ mysql_secure_installation
$ (/etc/init.d/mysql restart)

```

5. MariaDB User Config

```
$ mysql
mysql> CREATE USER alessio IDENTIFIED BY 'becode';
mysql> GRANT ALL PRIVILEGES ON *.* TO alessio;
mysql> exit

$ mysql -u alessio -p
mysql> CREATE DATABASE wordpress;
mysql> exit
```

6. Download Wordpress

```
$ cd /var/www/html/
$ wget https://fr.wordpress.org/latest-fr_FR.tar.gz
$ tar -xvzf latest-fr_FR.tar.gz
$ mv wordpress blog
$ rm latest-fr_FR.tar.gz
```

7. Save container to image

```
$ touch /run.sh
$ vim /run.sh

vim>
#!/bin/bash

/etc/init.d/mysql start
/usr/sbin/apache2ctl -D FOREGROUND
vim>

$ exit
$ docker commit XXXXXX wordpress
```

8. Start container with your wordpress image

```
$ docker run -d -p 5000:80 --name wordpress wordpress:latest sh /run.sh
$ docker exec -ti wordpress bash

```

**OPTIONAL USEFUL PHP EXTENSIONS**

```
$ apt -y install php-cli php-fpm php-json php-pdo php-mysql php-zip php-gd php-mbstring php-curl php-xml php-pear php-bcmath
```
