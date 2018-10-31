---
layout: post
title: "nodejs with mongo db app"
date: 2019-10-25 19:58:38 +0200
categories: nodejs
---

install mongodb
```
docker run --name test-mongo -v /tmp/test-mongo/datadir:/data/db -p 127.0.0.1:27017:27017 -d mongo
```

create an empty folder named "demo"
```
mkdir demo
```

initialize application:
```
npm init
```

install mongodb module
```
npm install mongodb --save
```



