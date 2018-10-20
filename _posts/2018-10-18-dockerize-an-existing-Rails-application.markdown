---
layout: post
title: "Dockerize an existing Rails application"
date: 2018-10-20 15:58:38 +0200
categories: docker
---

crea un app in rails:
```
rails new demo -d mysql
```

create a Dockerfile:
```
FROM ruby:2.5.1
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs
RUN mkdir /usr/src/demo
WORKDIR /usr/src/demo
ADD Gemfile /usr/src/demo/Gemfile
ADD Gemfile.lock /usr/src/demo/Gemfile.lock
RUN bundle install
ADD . /usr/src/demo
RUN RAILS_ENV=production bundle exec rake assets:precompile --trace
```

create docker-compose.yml
```
version: '2'
services:
  app:
    build: .
    command: bundle exec puma -C config/puma.rb
    volumes:
      - .:/usr/src/demo
    expose:
      - "3000"
    environment:
      RACK_ENV: production
      RAILS_ENV: production
    ports:
        - 3000:3000
	depends_on:
      - db
    links:
      - db
  
  db:
    image: "mysql:5.7"
    environment:
      - MYSQL_ROOT_PASSWORD=password
      - MYSQL_DATABASE=db
      - MYSQL_USER=root
      - MYSQL_PASSWORD=password
    ports:
      - "3306:3306"
  
  phpmyadmin:
    depends_on:
      - db
    image: phpmyadmin/phpmyadmin
    restart: always
    ports:
      - 8081:80
    environment:
      PMA_HOST: db
      MYSQL_ROOT_PASSWORD: password
```