---
layout: post
title: "kong api gateways with docker"
date: 2018-10-13 15:58:38 +0200
categories: docker
---
[KONG](https://konghq.com/) is a api gateway placed above the microservices aimed to managing the APIs and interactions external and internal with them in a more rational, efficient, safe and "easy" way, helping the developer in the separation between public and private APIs, improving their performance , facilitating monitoring etc ...

### Some Characteristic:
- Built on NGINX
- For the most part written in LUA
- Expandable via many Plugins
- It supports two types of Datastores, which can also be used simultaneously (Postgres, cassandra)

### Advantages:
- Easy management of APIs in a Microservice architecture
- helps in the separation between Public and Private APIs
- allows to measure and possibly limit the use of these APIs.
- Improves performance / through the use of caching etc ...)
- Expandable through many plugins

The best way for move the fists step with KONG is using docker:

first, create and internal network for the dockers:
```
docker network create kong-net
```
create a container for cassandra db:
```
docker run -d --name kong-cassandra-database \
              --network=kong-net \
              -p 9042:9042 \
              cassandra:3
```

create a postgre sql container:
```
docker run -d --name kong-postgres-database \
              --network=kong-net \
              -p 5432:5432 \
              -e "POSTGRES_USER=kong" \
              -e "POSTGRES_DB=kong" \
              postgres:9.6
```

create a migration:
```
docker run --rm \
    --network=kong-net \
    -e "KONG_DATABASE=postgres" \
    -e "KONG_PG_HOST=kong-postgres-database" \
    -e "KONG_CASSANDRA_CONTACT_POINTS=kong-postgres-database" \
    kong:latest kong migrations up
```

create a kong container connected to the postgres and cassandra databases throught the internal network kong-net:
```
docker run -d --name kong \
    --network=kong-net \
    -e "KONG_DATABASE=postgres" \
    -e "KONG_PG_HOST=kong-postgres-database" \
    -e "KONG_CASSANDRA_CONTACT_POINTS=kong-cassandra-database" \
    -e "KONG_PROXY_ACCESS_LOG=/dev/stdout" \
    -e "KONG_ADMIN_ACCESS_LOG=/dev/stdout" \
    -e "KONG_PROXY_ERROR_LOG=/dev/stderr" \
    -e "KONG_ADMIN_ERROR_LOG=/dev/stderr" \
    -e "KONG_ADMIN_LISTEN=0.0.0.0:8001, 0.0.0.0:8444 ssl" \
    -p 8000:8000 \
    -p 8443:8443 \
    -p 8001:8001 \
    -p 8444:8444 \
    kong:latest
  ```

check on url http://localhost:8001/

For more info, go on official web site [here](https://konghq.com/)

<iframe width="560" height="315" src="https://www.youtube.com/embed/wK0obqAFut0" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
