---
layout: post
title: "Joomla in Docker with Mysql"
date: 2018-10-05 11:58:38 +0200
categories: docker
---

Create a mysql database container:
```
docker run --name mysql-joomla -e MYSQL_ROOT_PASSWORD=password123 -d mysql:5.7
```

go inside mysql containers and create an empty database:
````
docker exec -it test-mysql bin/bash

mysql -u root -p
```

create the db:
```
CREATE DATABASE joomlademo;
```
check if db was created:
```
show databases;
```

exit from mysql and from container:

create the joonla container linked to mysql container and with a volume for can edit or add files:
```
docker run --name joomla-demo \
  --link mysql-joomla:db \
  -e JOOMLA_DB_HOST=db:3306 \
  -e JOOMLA_DB_USER=root \
  -e JOOMLA_DB_PASSWORD=password123 \
  -v /tmp/test-joomla:/var/www/html -p 8080:80 -d joomla
```

connect to http://0.0.0.0:8080

and follow the wizard installation of joomla, using "db" and not localhost ad database host.

<iframe width="560" height="315" src="https://www.youtube.com/embed/ne4tGOqSNlo" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
