---
layout: post
title: "MongoDB with Docker"
date: 2018-10-25 10:58:38 +0200
categories: golang
---

create MongoDB container:
```
docker run --name test-mongo -p 8081:8081 -d mongo

docker run --name test-mongo -v /tmp/test-mongo/datadir:/data/db -p 8081:8081 -d mongo

```

enter in containner:
```
docker exec -it test-mongo bash
```

launch mongo console:
```
mongo
```

create a db:
```
use demo
```

show all databases (you will see demo database only when you will insert at least one data)
```
show dbs
```

insert data:
```
db.agenda.insert({"nome":"napoleone", "cognome":"Bonaparte"})
```

see all data
```
db.agenda.find()
```

simple query:
```
db.agenda.find({nome: "Giuseppe"})
```

