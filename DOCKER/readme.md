# Create a Wordpress environemment on DOCKER

## What's & Why DOCKER ?

Create containers with operating system, wich version of tools you need, you can do some benchmark easily to determine the best tools for your software...

## What we need ?

- OS : **debian**, centOs, Ubuntu Server ..
- HTTP Server : **apache2**, nginx ..

- For Wordpress :
  - PHP
  - MySql
  - [Wordpress](https://fr.wordpress.org/download/)

## Let's Go

1. Install DOCKER

2. Get the [debian](https://hub.docker.com/_/debian?tab=description) image

```shell
$ docker pull debian:latest
```

3. Create and access Debian container

```shell
$ docker run -t -i debian:latest
```

4. Install tools and apache2

```
$ apt update && apt -y upgrade

$ apt -y install wget vim apache2
$ apt -y install php php-common libapache2-mod-php
$ a2enmod php7.3

$ apt -y install mariadb-server php-mysql
$ mysql_secure_installation
$ (/etc/init.d/mysql restart)

**OPTIONAL USEFUL PHP EXTENSIONS**
$ apt -y install php-cli php-fpm php-json php-pdo php-mysql php-zip php-gd  php-mbstring php-curl php-xml php-pear php-bcmath

$ exit
```

5. Save container to image

```
$ docker commit XXXXXX wordpress
```

6. Start container with your wordpress image

```
$ docker run -d -p 5000:80 --name wordpress wordpress:latest /usr/sbin/apache2ctl -D FOREGROUND
$ docker exec -ti wordpress bash

$ echo "<?php phpinfo(); ?>" | tee /var/www/html/phpinfo.php
```

7. Download Wordpress

```
$ cd /var/www/html/
$ wget https://fr.wordpress.org/latest-fr_FR.tar.gz
$ tar -xvzf latest-fr_FR.tar.gz
$ mv wordpress blog
$ rm latest-fr_FR.tar.gz
```

8. MariaDB User Config

```
$ mysql
mysql> CREATE USER alessio IDENTIFIED BY 'becode';
mysql> GRANT ALL PRIVILEGES ON *.* TO alessio;
mysql> exit

$ mysql -u alessio -p
mysql> CREATE DATABASE wordpress;
mysql> exit
```

9. Open your browser to config your site

To get your ip address

```
$ ip address show
```

Open your [website](http://172.X.0.X/blog)
