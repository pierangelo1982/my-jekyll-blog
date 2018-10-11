---
layout: post
title: "wordpress in Docker with Mysql"
date: 2018-10-05 11:58:38 +0200
categories: docker
---

Create a mysql database container:
```
docker run --name mysql-wordpress -e MYSQL_ROOT_PASSWORD=password123 -d mysql:5.7
```

go inside mysql containers and create an empty database:
````
docker exec -it test-mysql bin/bash

mysql -u root -p
```

create the db:
```
CREATE DATABASE wordpressdemo;
```
check if db was created:
```
show databases;
```

exit from mysql and from container:

create the wordpress container linked to mysql container and with a volume for can edit or add files:
```
$ docker run --name wordpress-demo \
  --link mysql-wordpress:db \
  -v /tmp/wordpress/wp-content:/var/www/html/wp-content \
  -p 8080:80 -d wordpress
```

connect to http://0.0.0.0:8080

and follow the wizard installation of wordpress, using "db" and not localhost ad database host.

<iframe width="560" height="315" src="https://www.youtube.com/embed/z2qzCS2RQXA" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
