---
layout: post
title: "Install Redmine with Docker"
date: 2018-23-20 10:58:38 +0200
categories: docker
---

Redmine is a project management web application written in Ruby on Rails, is cross-platform and cross-database (mysql, PostgreSQL).

create a mysql container:
```
docker run -d --name redmine-mysql -e MYSQL_ROOT_PASSWORD=password123 -e MYSQL_DATABASE=redmine mysql:5.7
```

create redmine:
```
docker run -d --name demo-redmine -v /tmp/redmine-demo/datadir:/usr/src/redmine/files --link redmine-mysql:mysql -p 3000:3000 redmine
```

