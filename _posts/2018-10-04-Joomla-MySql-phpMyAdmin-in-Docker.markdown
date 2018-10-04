---
layout: post
title:  "Joomla + MySql + phpMyAdmin in Docker"
date: 2018-10-04 19:00:38 +0200
categories: docker
---

Ok. I’m a novice of Docker, so probably exists better ways for run Joomla + MySql + phpMyAdmin on Docker, I dont know, but for now this is my way:

### MYSQL
First step open the terminal and download MySql Docker Image:
```
docker pull mysql
```
After we can start our instance, we will named it test-mysql, root as user and 12345678 as password.

After we must to create a “volumes”, a local folder from the host on the container to store the database files.
```
docker run --name test-mysql -v /Your/LocalPath/Desktop/mysql-nginx/datadir:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=12345678 -d mysql
```
By default, our mysql should be run on port 3306.

### phpMyAdmin
Download phpMyAdmin Docker Image
```
docker pull phpmyadmin/phpmyadmin
```
Now we start the instance, naming it test-phpmyadmin and link it to the database mysql test-mysql that we have created before, and run it on the port 8084.
```
docker run --name test-phpmyadmin -d --link test-mysql:db -p 8084:80 phpmyadmin/phpmyadmin
```
Now, if we connect with our browser at the address http://localhost:8084 we should have phpMyAdmin login page, and we can access with username: root and password: 12345678

![image-title-here](/img/2018-10-04-Joomla-MySql-phpMyAdmin-in-Docker/phpmyadmin-login.png){:class="img-fluid"}

### Joomla
Ok, now it’s the turn of Joomla, as already done before, we must download the image:
```
docker pull joomla
```
Now we start the instance on the port 8080 with the name of test-joomla, link it at the database and create a volumes.
```
docker run --name test-joomla --link test-mysql:db -e JOOMLA_DB_HOST=db:3306 -e JOOMLA_DB_USER=root -e JOOMLA_DB_PASSWORD=12345678 -v /your/local/path/Desktop/joomla-docker/joomla_www:/var/www/html -p 80:80 -d joomla
```
if it’s all correct, at the address [http://localhost](http://localhost) we should find our joomla installation.